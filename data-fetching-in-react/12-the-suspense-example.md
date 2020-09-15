# Et eksempel med SWR og Suspense

```tsx
// App.tsx
import React from "react";
import { Router, Route } from "react-router-dom";
import { ErrorBoundary } from "react-error-boundary";
import { PostPage } from "./PostPage";
import { ProfilePage } from "./ProfilePage";

const App = () => {
  return (
    <Router>
      <React.Suspense fallback={<Spinner />}>
        <ErrorBoundary fallback={<p>Oops, something crashed</p>}>
          <Route path="/post/:id" component={PostPage} />
          <Route path="/profile" component={ProfilePage} />
        </ErrorBoundary>
      </React.Suspense>
    </Router>
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
  const { data: post } = useSWR(
    ["post", postId],
    api.getPost,
    { suspense: true }
  );

  return (
    <Page>
      <h1>{post.title}</h1>
      <Meta {...post.meta} />
      <Content content={post.content}>
    </Page>
  );
};
```
