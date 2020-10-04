# Eksempel med en spesifisert fetcher

```tsx
// App.tsx
import React from "react";
import { Router, Route } from "react-router-dom";
import { ErrorBoundary } from "react-error-boundary";
import { SWRConfig } from "swr";
import { PostPage } from "./PostPage";
import { ProfilePage } from "./ProfilePage";

const getJson = async (url: string, options?: FetchOptions) => {
  const response = await fetch("/api/" + url, options);
  const body = await respons.json();
  if (!response.ok) {
    throw Error(body);
  }
  return body;
};

const App = () => {
  return (
    <SWRConfig value={{ suspense: true, fetcher: getJson }}>
      <Router>
        <React.Suspense fallback={<Spinner />}>
          <ErrorBoundary fallback={<p>Something crashed</p>}>
            <Route path="/post/:id" component={PostPage} />
            <Route path="/profile" component={ProfilePage} />
          </ErrorBoundary>
        </React.Suspense>
      </Router>
    </SWRConfig>
  );
};
```

```tsx
import React from "react";
import useSWR from "swr";
import { useParams } from "react-router-dom";
import * as api from "./api";

const PostPage = () => {
  const { postId } = useParams();
  const { data: post } = useSWR<Post>(`post/${postId}`);

  return (
    <Page>
      <h1>{post.title}</h1>
      <Meta {...post.meta} />
      <Content content={post.content}>
    </Page>
  );
};
```
