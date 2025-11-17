# [TODO] Angular Global Error Handling

## Problem
I used to scatter `catch` blocks and `alert()` calls across routable components.   That’s noisy, unmaintainable, and hides cross-cutting logic inside feature code.

## Conceptual fix
Angular already provides a unified hook for handling **any uncaught error**:  the [`ErrorHandler`](https://angular.io/api/core/ErrorHandler) interface.

Instead of catching errors in each component:
1. Generate a handler class.
   ```bash
   ng g service core/global-error-handler
   (Angular may create it as global-error-handler.ts; that’s fine.)
   ```
2. Implement handleError() to log and/or display alerts, toasts, etc.
3. Register it globally so Angular routes all unhandled exceptions through it.

## Registration

Add to providers (standalone config or AppModule):

```
{ provide: ErrorHandler, useClass: GlobalErrorHandler }
```

If using standalone bootstrap, put it in app.config.ts inside ApplicationConfig.providers.

## Optional refinements

* (If I have a server-side) Forward HTTP errors from interceptors by calling ErrorHandler.handleError(error).
* Replace `alert()` with a proper notification or logging service later.

## Outcome

One central place handles every unexpected error.
Component code stays clean; diagnostics stay visible.