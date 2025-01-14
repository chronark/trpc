---
id: merging-routers
title: Merging Routers
sidebar_label: Merging Routers
slug: /merging-routers
---


Writing all API-code in your code in the same file is not a great idea. It's easy to merge routers with other routers. 

Thanks to TypeScript 4.1 template literal types we can also prefix the procedures without breaking type safety.

## Working example

- Code at [/examples/prisma-starter/pages/api/trpc/%5Btrpc%5D.tsx](https://github.com/trpc/trpc/blob/main/examples/prisma-starter/pages/api/%5Btrpc%5D.tsx)
- All code for posts living in a separate router and namespaced with `posts.`
- Live at [hello-world.trpc.io](https://hello-world.trpc.io)


## Example code


```ts
const posts = createRouter()
  .mutation('create', {
    input: z.object({
      title: z.string(),
    }),
    resolve: ({ input }) => {
      // ..
      return {
        id: 'xxxx',
        ...input,
      }
    },
  })
  .query('list', {
    resolve() {
      // ..
      return []
    }
  });

const users = createRouter()
  .query('list', {
    resolve() {
      // ..
      return []
    }
  });


const appRouter = createRouter()
  .merge('users.', users) // prefix user procedures with "users."
  .merge('posts.', posts) // prefix poosts procedures with "posts."
  ;
```
