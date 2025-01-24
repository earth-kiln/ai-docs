```realtime.tsx
import { createBrowserClient } from '@/core/supabase/browser';
import { Tables } from '@/core/supabase/database';
import { CURRENT_SCHEMA } from '@/core/supabase/schema';
import { RealtimePostgresChangesPayload } from '@supabase/supabase-js';
import { useEffect, useState } from 'react';

export function DemoSubscription() {
  const supabase = createBrowserClient();
  const [data, setData] = useState([]);

  useEffect(() => {
    const getData = async () => {
      const { data } = await supabase.from('example').select('*');
      setData(data ?? []);
    };

    getData();

    const subscription = supabase
      .channel('example')
      .on(
        'postgres_changes',
        { event: '*', schema: CURRENT_SCHEMA, table: 'example' },
        (payload: RealtimePostgresChangesPayload<Tables<'example'>>) => {
          getData();
        }
      );

    subscription.subscribe();
  }, [supabase]);

  return <div>{JSON.stringify(data)}</div>;
}
```
