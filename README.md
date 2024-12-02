# EAI SvelteKit Starter Project

Welcome to the EAI SvelteKit Starter Project, crafted using Svelte 5 and SvelteKit with Tailwind CSS and Prettier and ESLint. This setup is designed to facilitate the development of enterprise-grade web applications by leveraging the efficiencies of Svelte, a framework that compiles away to produce highly efficient vanilla JavaScript.

## Getting Started

### Prerequisites
- Node.js (23.3.0)
- npm

## Cloning and Renaming the App for a New Project

Follow these steps to clone this repository and set it up for use in a different project:

1. **Clone the Repository:**
   Open a terminal and run the following command to clone the repository:
   ```
   git clone git@github.com:eaiti/eai-sveltekit-starter.git
   ```

2. **Navigate to the Project Directory:**
   Move into the newly cloned repository:
   ```
   cd eai-sveltekit-starter
   ```

3. **Remove the Existing Git History:**
   If you want to start fresh with a new Git history for your project, remove the existing `.git` folder:
   ```
   rm -rf .git
   ```

4. **Reinitialize Git:**
   Reinitialize Git for your new project:
   ```
   git init
   ```

5. **Rename the Project:**
   Update the project name and description in the `package.json` file:
   ```
   {
     "name": "new-project-name",
     "version": "1.0.0",
     "description": "A brief description of your new project",
     ...
   }
   ```

6. **Install Dependencies:**
   Install the necessary dependencies for the project:
   ```
   npm install
   ```

7. **Update the Project Name in Configuration Files:**
   - Check for occurrences of the old project name in files like `README.md`, `.eslintrc.cjs`, or `tailwind.config.cjs` and update them accordingly.
   - Replace `eai-sveltekit-starter` or similar placeholders with your new project name.

8. **Commit the Initial State:**
   After making all changes, commit the new project's initial state:
   ```
   git add .
   git commit -m "Initial commit for new project"
   ```

9. **Link to a New Remote Repository (Optional):**
   If you plan to push this new project to a Git repository, link it to the new remote repository:
   ```
   git remote add origin https://your-new-repo-url.git
   git branch -M main
   git push -u origin main
   ```

10. **Run the Application:**
    Start the development server to confirm that everything works:
    ```
    npm run dev
    ```

Your new project is now set up and ready for development!



### Running the Application
To launch the development server:
``` npm run dev ```
Visit http://localhost:3000 in your browser. The app will automatically update as you modify the source files.

## Adding New Components

To create new components in SvelteKit, here’s an example of a **Parent** and **Child** component interaction showcasing inputs (props) and outputs (events) using TypeScript.

### Child Component

Create a file `src/lib/components/ChildComponent.svelte`:

```svelte
<script lang="ts">
	// Props (input from parent)
	type childProps = {
		initialCount: number;
		currentCount: (count: number) => void;
	};

	let { initialCount, currentCount }: childProps = $props(); // instead of `export let`

	// Reactive state
	let count = $state<number>(initialCount);

	// Derived state: double the count
	const doubleCount = $derived(count * 2);

	// Effect: Log an event whenever `count` changes, set output prop
	$effect(() => {
		currentCount(count);
		console.log('Count changed:', count);
		console.log('Double Count:', doubleCount);
	});

	// Functions to update the state
	function increment(): void {
		count += 1;
	}

	function decrement(): void {
		count -= 1;
	}
</script>

<div>
	<h3>Child Component</h3>
	<p>Count: {count}</p>
	<p>Double Count: {doubleCount}</p>
	<button onclick={increment}>Increment</button>
	<button onclick={decrement}>Decrement</button>
</div>

<style>
	button {
		margin-right: 0.5rem;
	}

	div {
		padding: 1rem;
		border: 1px solid #ccc;
		border-radius: 8px;
		margin-bottom: 1rem;
	}
</style>
```

### Parent Component

Create a file `src/lib/components/ParentComponent.svelte`:


```svelte
<script lang="ts">
	import ChildComponent from './ChildComponent.svelte';

	// Handler for receiving updates from the child
	function handleUpdate(count: number): void {
		console.log('Parent received:', count);
	}
</script>

<div class="parent">
	<h2>Parent Component</h2>
	<ChildComponent initialCount={5} currentCount={handleUpdate} />
</div>

<style>
	.parent {
		border: 2px dashed #4caf50;
		padding: 1rem;
		border-radius: 8px;
		margin-top: 1rem;
	}
</style>
```

### Explanation

1. **Child Component:**
   - The child component receives `initialCount` as a prop with the type `number`.
   - It initializes its `count` state using Svelte's `state` rune and calculates `doubleCount` using the `derive` rune.
   - The child emits an `update` event using Svelte's `createEventDispatcher` with a typed payload that includes `count` and `doubleCount`.
   - The child component provides functions to increment and decrement the `count` state, which in turn updates the derived `doubleCount`.

2. **Parent Component:**
   - The parent listens for the `update` event from each child component and uses TypeScript to ensure the event's `detail` object has the correct shape (containing `count` and `doubleCount`).
   - Multiple instances of the child component are created, each with an independent `initialCount` prop, which initializes its internal state.

3. **Behavior:**
   - Interacting with the buttons in a child component updates its internal `count` state.
   - Whenever the state changes, the child emits an `update` event to the parent, passing the current `count` and `doubleCount`.
   - The parent logs the `update` event's details for each child instance independently.



## Adding New Routes
In SvelteKit, routes are determined by the structure of the src/routes folder. For instance:
- Creating src/routes/about.svelte corresponds to accessing /about.

To add a route:
1. Add a new Svelte file in src/routes, e.g., contact.svelte.
2. Define the route with Svelte syntax:
   <script>
     let message = 'Contact us here!';
   </script>

   <style>
     h1 { color: blue; }
   </style>

   <h1>{message}</h1>

## Similarities with Angular and React
- Component Structure: Svelte shares the component-based structure seen in Angular and React, promoting code reusability.
- Reactivity: Unlike React's hooks or Angular's observables, Svelte achieves reactivity through simple assignments, which can be more intuitive.
- Routing: Like Angular and React (with React Router), SvelteKit handles routing declaratively, supporting both nested and dynamic routes.

