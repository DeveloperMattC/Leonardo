# Prefer built-in control flow over `*ngIf`

Do not use `*ngIf`, `*ngFor`, or `*ngSwitch`. Use Angular built-in control flow instead.

## `@if` instead of `*ngIf`

```html
<!-- Do not -->
<section *ngIf="isReady">...</section>
<section *ngIf="isReady; else loading">...</section>

<!-- Do this -->
@if (isReady) {
  <section>...</section>
} @else {
  <p>Loading</p>
}
```

## `@for` instead of `*ngFor`

```html
<!-- Do not -->
<li *ngFor="let item of items">{{ item.name }}</li>

<!-- Do this -->
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
} @empty {
  <li>No items</li>
}
```

## `@switch` instead of `*ngSwitch`

```html
<!-- Do not -->
<div [ngSwitch]="status">
  <p *ngSwitchCase="'ok'">Ready</p>
  <p *ngSwitchDefault>Unknown</p>
</div>

<!-- Do this -->
@switch (status) {
  @case ('ok') {
    <p>Ready</p>
  }
  @default {
    <p>Unknown</p>
  }
}
```

Do not import `NgIf`, `NgFor`, or `NgSwitch` into components for these cases.
