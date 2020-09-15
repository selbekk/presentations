# Et eksempel med SWR og parametre

```ts
// api.ts
export const getPost = await (cacheName: string, postId: string) => {
  const response = await fetch(`/api/post/${postId}`);
  if (!response.ok) {
    throw Error(`Post with ID ${postId} not found`);
  }
  return response.json() as Post;
}
```

```tsx
import React from "react";
import useSWR from "swr";
import { useParams } from "react-router-dom";
import * as api from "./api";

const PostPage = () => {
  const { postId } = useParams();
  const { data: post, isValidating, error } = useSWR(
    ["post", postId],
    api.getPost
  );

  if (!post && isValidating) {
    return <Spinner />;
  }

  if (error) {
    return <p>Oops, something crashed</p>;
  }

  return (
    <Page>
      <h1>{post.title}</h1>
      <Meta {...post.meta} />
      <Content content={post.content}>
    </Page>
  );
};
```
