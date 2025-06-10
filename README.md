# bertram-coffee-break

This  project is built using Vue 3 in Vite.

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Local Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Architecture
I chose a very simple architecture, a Vue front-end with no backend.

I considered using a backend API with a database, simply to showcase the capability,
but it seemed overkill for this scope.

Instead, we are using local storage to manage memory between browser sessions.

### Algorithm
My application allows changing the price of drinks per round; while not required
by the scope provided, I've allowed this to exhibit the flexibility of the algorithm.

The algorithm tracks a Tab for each team member, which is updated each round.

The highest total Tab determines who pays the current round, and the price of the round
is removed from their Tab.

This approach accounts for several edge cases, like large price ranges between team members,
allowing someone to "skip" a round, and adjusting the cost of drinks between rounds.

In addition, we are tracking total rounds bought and total amount paid by each team member.

### Interface
Vue is used to power a single-page application consisting of a single form.

All form fields and logic are self-contained in its own component.

Prices for each team members' drinks can be adjusted for each round.

Clicking the button calculates the updated tabs of all team members,
and selects the member with the highest tab to pay for the round.

Refreshing the page maintains the existing data.

A Reset button is provided to return the system to its initial state.

### Assumptions
The names and number of the team members who participate in the Bertrand Coffee Break are fixed.

This can be made user-editable, and allow changing who participates in each round,
but would make the application considerably more complex, and as I was asked to only spend
a couple hours on this project, I have chosen to leave the team members dev-configurable.

The configuration can be adjusted in the DrinkPricesForm.vue file - the employee names
are used as keys to storage objects.

Due to us using local storage, the application needs to be run on the same
computer and the same browser each time. A more flexible memory can be attained with
a backend file system or database that can be loaded across multiple browser sessions.



