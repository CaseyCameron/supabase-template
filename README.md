This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

# To Do App in Next.js for [alchemycodelab](https://www.alchemycodelab.com/)

This is a no frills, unstyled, _row level security_ secured To Do app built with Supabase and Next.js

Please follow the following steps to get setup.

For help, the fastest way to reach me is via twitter [@jsummersmuir](https://twitter.com/jsummersmuir).
Please also feel free to send me feedback (good **and** bad) to me about your experience with Supabase. We'd love to improve anything you're struggling with.

## Create a Supabase project

Go to [app.supabase.io](https://app.supabase.io) and create an account.
_(You'll need a [GitHub](https://github.com) account)_

Then create a new project, make sure to pick a strong password.
_(Don't worry, you can [reset the password](https://supabase.io/docs/guides/database/managing-passwords) later if you need to.)_

## Configure your new Supabase project

1. **Add schema and policies**

You may have watched me (@MildTomato) earlier in the hackathon making the table and policies using the supabase dashboard.

You may not remember what I did, but not to worry, you can just run the below in the SQL editor, to add all the things you need to get going.

So in your project click on 'SQL Editor' in the left side nav bar, and then click 'New query'.

Below is the SQL required to setup the project, it will create the table and also set up the RLS policies.
Copy the whole snippet and run it in the SQL editor.

```sql

-- Create a table called `tasks` for the tasks

CREATE TABLE tasks (
  id bigint GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
  user_id uuid REFERENCES auth.users NOT NULL,
  inserted_at timestamp with time zone DEFAULT timezone('utc'::text, now()) NOT NULL,
  updated_at timestamp with time zone DEFAULT timezone('utc'::text, now()) NOT NULL,
  task text,
  checked boolean DEFAULT false NOT NULL
)

-- Create policies for Row Level Security

CREATE POLICY "Enable INSERT for users whose uid equals user_id" ON public.tasks FOR
INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Enable SELECT for users whose uid equals user_id" ON public.tasks FOR
SELECT USING (auth.uid() = user_id);

CREATE POLICY "Enable UPDATE for users whose uid equals user_id" ON public.tasks FOR
UPDATE USING (auth.uid() = user_id) WITH CHECK (auth.uid() = user_id);

```

2. **Check that table and policies were added correctly**

Now, if you go to **Table Editor** (click on the Table Editor icon in the side nav bar) you'll be able to see the table called 'tasks'. There won't be any data in it yet as we haven't inserted any rows yet.

If you go to **Auth** (click on the Auth icon in the side nav bar) you'll be able to then click on **Policies**, and now you should see 3 new policies that can be used to protect your table with Row Level Security (RLS)

3. **Enable Row Level Security (RLS)**

In the **Policies** page, click on _Enable RLS_
Click _confirm_, and now your table is enforcing the policies we've just added.

4. **Turn off Email confirmations**

Click on **settings** in **Auth** and disable 'Enable email confirmations'.

This stops users having to click on a confirmation link in an email when signing up.
It's highly recommended you use this in production, but for testing purposes it can be a nuisance.

## Running the client

1. **Add Supabase URL and Anon Key**

Go to **Settings** (click on the Settings cog icon in the side nav bar), and then click **API**.

You'll see an `anon` key, and also a `url`.

Open the .env file in the root of the todo app, and add `anon` key and `url` values for the following:

```
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_ANON_KEY
```

If you have got ahead of yourself (naughty..) and already run `npm run dev`, you will now need to restart the app so these new env variables take effect.

2. **Install dependencies and run client**

Open you're favourite terminal, go to the folder of this project and run:

```bash
npm install
```

then you can run the app

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

## Learn More about Nextjs

We have used Next.js for this example.

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.
