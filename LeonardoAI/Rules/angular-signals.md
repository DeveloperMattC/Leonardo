# Favor signals over observables

Use Angular signals for component and service state. Do not introduce `Observable`, `BehaviorSubject`, or `async` pipe for new UI state.

## Component state

```typescript
// Do not
generatedString = '';
ready$ = new BehaviorSubject(false);

// Do this
generatedString = signal('');
isReady = computed(() => this.generatedString().length > 0);
```

Read signals in templates as `generatedString()`, not through `| async`.

## Inputs and outputs

Prefer `input()`, `output()`, and `model()` over `@Input()` and `@Output()`.

## When an observable still exists

Keep RxJS at the boundary (HttpClient, router events, third-party streams). Convert immediately with `toSignal()`, or use `resource()` / `rxResource()` when it fits Angular 19.

Do not `subscribe()` in components to drive template state. Do not add `NgIf` plus `async` pipe for that state.

Promises from existing services can stay as `async`/`await`. Store the result in a signal.
