# Forms and Inputs

The Partner With Us contact form and the Careers job search are the verified
sources. Everything else is a **Proposed** extension using the same tokens.

## Verified form anatomy (Partner With Us)

```html
<form class="flex flex-col gap-6">
  <div class="grid md:grid-cols-2 gap-4">
    <div class="flex flex-col gap-1">
      <label class="text-xs block text-left">Full Name <span class="text-[#D92D20]">*</span></label>
      <input name="fullName" placeholder="Enter your full name"
             class="border border-[#D0D5DD] focus:border-[#50A6D8] focus:border-[2.5px] h-[44px] text-sm rounded-lg p-3 w-full outline-none" />
    </div>
    …
  </div>
  <textarea name="message" placeholder="Enter your message"
            class="resize-none text-sm rounded-lg p-3 w-full my-0.5 border border-[#D0D5DD] focus:border-[#50A6D8] focus:border-[2.5px] outline-none"></textarea>
  <div class="text-xs">0/1000</div>   <!-- character counter -->
  <button type="submit" class="bg-blue text-white px-8 py-3 font-bold rounded-lg …">Send Message</button>
</form>
```

Spec table:

| Property | Verified value |
|---|---|
| Form container | White card `p-10`, title "Contact Us" 20px 700, subtitle 16px 500 |
| Field stack | `gap-6` (24px); related fields in `grid md:grid-cols-2 gap-4` |
| Label | 12px, 400, above the field, 4px gap; required mark `*` in `#D92D20` |
| Input | 44px tall, 12px padding, 14px text, 1px `#D0D5DD` border, 8px radius, white background, placeholder `#667085` |
| Focus | border `#50A6D8` at 2.5px, `outline: none` |
| Textarea | same, `resize-none`, with a `n/1000` counter below |
| Submit | primary button, left-aligned under the fields, full width on mobile |
| Search field (Careers) | borderless input `text-sm leading-5 outline-none focus:ring-0` inside a container, placeholder "Search Jobs", text `#040815` |

## Proposed product form standards

### Controls

| Control | Spec |
|---|---|
| Text input / number / date | As verified (44px). A 36px `sm` size (`h-9 text-sm px-3`) is allowed in dense tables and filter bars |
| Select | Same frame as input, Heroicons `ChevronDownIcon` 20px `grey` at right (12px inset); use a native `<select>` unless search/multi-select is needed, then a listbox component from the host project |
| Textarea | Min 3 rows, `resize-y` in products (site uses `resize-none` for the fixed contact form) |
| Checkbox | 16px, 4px radius, border `#D0D5DD`, checked `bg-blue` with white `CheckIcon` (the site's Tailwind forms plugin uses `checked:bg-blue`) |
| Radio | 16px, `rounded-full`, checked `border-[5px] border-blue` |
| Toggle switch | 36 × 20px track `bg-gray-300`, on `bg-blue`, 16px white knob, 150ms |
| Search field | Input with `MagnifyingGlassIcon` 20px `grey` left (12px inset), `pl-10`, clear button when non-empty, Enter submits, debounce 300ms for live search |
| File upload | Dashed 1px `#50A6D8` border, 8px radius, `bg-light-blue`, 24px `ArrowUpTrayIcon`, "Drag files here or browse" |
| Date range | Two inputs or a single trigger showing "01 Jan 2026 – 31 Jan 2026"; calendar panel styled like a dropdown panel |

### Labels and help

- Labels always visible (no placeholder-only fields), 12px 500 in products (`text-xs font-medium text-black`), sentence case.
- Required: `*` in `error-red` after the label; alternatively mark optional fields "(optional)" when most fields are required.
- Help text: 12px `grey` under the field, 4px gap, linked with `aria-describedby`.
- Placeholder: example value only ("email@domain.com"), never instructions.

### Layout

- Single column by default, max width 640px; two columns (`md:grid-cols-2`) only for short related fields (first/last name, city/postcode), as verified.
- Group related fields under an `h3` (16px 600) with 32px above the group.
- Actions: left-aligned under the form on marketing pages (verified); **right-aligned** in dialogs and admin forms (Proposed), secondary before primary, `gap-3`.
- Filter bars (dashboards): horizontal row of `sm` controls, wrap on mobile; "Apply" primary `sm` + "Reset" ghost. See [../examples/dashboard-guidelines.md](../examples/dashboard-guidelines.md#filters).

### Validation and error messages

- Validate on blur and on submit; never only on keystroke.
- Error field: border `error-red` 1px (2px on focus), message 12px `error-red` under the field with `ExclamationCircleIcon` 16px, `aria-invalid="true"`, `aria-describedby` pointing at the message.
- Message copy: say what is wrong and how to fix it ("Enter a valid email address, like name@company.com"). No exclamation marks, no "Invalid input".
- Form-level summary on submit failure: inline error alert at the top listing fields, each a link that focuses the field.
- Success: inline success alert or toast ("Message sent. We'll get back to you soon!" mirrors the site's tone), then clear or disable the form.
- Character counters: `n/max` 12px `grey`, turns `error-red` at the limit (verified pattern `0/1000`).

### States

Default, hover (border `dark-grey`-tinted `#B8BEC9` proposed), focus (verified `#50A6D8`), filled, disabled (`bg-gray-100 text-light-grey border-[#D0D5DD] cursor-not-allowed`), read-only (`bg-light-blue`, no border change), error, success (optional green border `success-700`). Full list in [interaction-states.md](interaction-states.md#form-controls).

### Accessibility

- Every control has a `<label for>` or `aria-label`; groups use `<fieldset>` + `<legend>`.
- Focus must be visible: the 2.5px `#50A6D8` border is the verified focus style; add `focus-visible:ring-2 ring-blue ring-offset-2` for checkboxes, radios, toggles and buttons where no border exists.
- Keyboard: Tab order follows the visual order; Enter submits; Esc closes pickers.
- Touch targets ≥ 44px on mobile (inputs already are).

## Related files

- [components.md](components.md#buttons-verified) — submit and cancel buttons.
- [interaction-states.md](interaction-states.md) — all control states.
- [accessibility.md](accessibility.md) — contrast of borders and error text.
