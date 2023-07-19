# Disabling user interactions

Two common ways to disable user interactions come from manipulating the properties below.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/user-select */
user-select: none;
/* or */
/* https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events */
pointer-events: none;
```

Key differences to keep in mind

| Interaction              | user-select | pointer-events |
| ------------------------ | :---------: | :------------: |
| Select text              |     ❌      |       ✅       |
| Hover effects            |     ✅      |       ❌       |
| Right click context menu |     ✅      |       ❌       |

Other notes

- `user-select` by specification is not an inherited property but behaves as such in certain browsers
- setting `pointer-events` to `none` to go "through" the element and "underneath" it
- even after disabling `pointer-events` standard bubbling event behavior still applies if the child element has `pointer-events` enabled
- elements with `pointer-events: none` will still receive focus from keyboard navigation thus it should not be used as a replacement for the `disabled` attribute (it only prevents mouse/tap events)
