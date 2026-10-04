# Oracle APEX – Conditional Read-Only Column with jQuery

Make one Interactive Grid column **read-only based on the value of another column**, using a few lines of jQuery.

**Example rule:** if an employee's salary (`SAL`) is **2000 or more**, the commission (`COM`) field becomes read-only. Otherwise it stays editable.

## Overview

Oracle APEX lets you set a column as editable or read-only, but it does not directly support a rule such as "lock this column when another column meets a condition". This solution handles that with a small jQuery snippet that runs in a Dynamic Action.

## Features

- Conditional read-only logic between two grid columns
- No plugins or external libraries, only jQuery bundled with APEX
- Easy to adapt: change the column static IDs and the condition
- Works inside an Interactive Grid on the cell being edited

## Prerequisites

- Oracle APEX (tested on: _<your APEX version>_)
- An Interactive Grid with the columns you want to control (this example uses `EMP` with `SAL` and `COMM`)

## Setup

### Step 1: Assign Static IDs to the columns

In **Page Designer**, open the Interactive Grid and select each column:

| Column | Static ID |
|--------|-----------|
| Salary | `SAL` |
| Commission | `COM` |

(Under **Advanced → Static ID**.)

### Step 2: Create a Dynamic Action

1. Select the **Salary (SAL)** column in the Interactive Grid.
2. Create a Dynamic Action:
   - **Event:** Change
   - **Selection Type:** Column(s) → `SAL`
   - **True Action:** Execute JavaScript Code
3. Paste this code:

```javascript
if ( $('#SAL').val() >= 2000 ) {
    $("#COM").prop('readonly', true);
} else {
    $("#COM").prop('readonly', false);
}
```

### Step 3 (optional): Also run when the cell is first edited

So the rule is applied when a user opens a row, not only after changing salary, add a second Dynamic Action on the grid region with **Event:** Selection Change (or Page Load), using the same JavaScript code.

## How It Works

1. Each column's Static ID becomes the HTML element ID of its input when the cell is in edit mode.
2. The script reads the current value of `#SAL`.
3. If the salary is 2000 or more, it sets the `readonly` property on `#COM`.
4. If not, it removes the `readonly` property so the user can edit commission.

## Customizing

Change the IDs and condition to fit your own columns:

```javascript
if ( $('#YOUR_CONDITION_COLUMN').val() >= YOUR_VALUE ) {
    $("#YOUR_TARGET_COLUMN").prop('readonly', true);
} else {
    $("#YOUR_TARGET_COLUMN").prop('readonly', false);
}
```

Tips:

- `.val()` returns text. For reliable numeric comparison, use `Number($('#SAL').val())`.
- For text conditions, compare strings, for example `$('#STATUS').val() === 'APPROVED'`.

## Important: Add Server-Side Validation

Read-only set with JavaScript is only a **user-interface control**. A user with browser developer tools can bypass it. Always enforce the same rule on the server, for example with:

- A PL/SQL validation or process on the Interactive Grid save
- A database trigger on the table

## Repository Structure

```
├── README.md
├── src/
│   └── readonly-cell.js
├── docs/
│   └── demo.gif
└── LICENSE
```

## Contributing

Issues and pull requests are welcome. Please include your APEX version and steps to reproduce any problem.

## License

Released under the MIT License. See `LICENSE` for details.

## Author

**_<Your Name>_**
_<LinkedIn / Blog / Email>_
