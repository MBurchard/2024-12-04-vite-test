# 2024-12-04-vite-test

This project has been created to demonstrate a bug in 'not clear at the moment'.

## Setup 
The command used was `pnpm create vite`.

> Project name: … 2024-12-04-vite-test \
> Select a framework: > Vue \
> Select a variant: > Customize with create-vue
>
> Vue.js - The Progressive JavaScript Framework
>
> Add TypeScript? … No / **Yes** \
> Add JSX Support? … **No** / Yes \
> Add Vue Router for Single Page Application development? … No / **Yes** \
> Add Pinia for state management? … **No** / Yes \
> Add Vitest for Unit Testing? … No / **Yes** \
> Add an End-to-End Testing Solution? > No \
> Add ESLint for code quality? > Yes \
> Add Prettier for code formatting? … **No** / Yes
>
> Scaffolding project in /Users/mburchard/Entwicklung/2024-12-04-vite-test...
>
> Done. Now run:
> 
> cd 2024-12-04-vite-test \
> pnpm install \
> pnpm dev

## The Test Case
I modified *HelloWorld.vue* to create the simplest possible test case.

The `replace` method is supported, and everything works as expected..

This version corresponds to the second commit.

Now I changed `replace` to `replaceAll`. This is also widely supported. However, you need to configure `"lib": ["ES2021"]`or later to use it.

The project generator created a *tsconfig.app.json* that extends *@vue/tsconfig/tsconfig.dom.json* which uses **ES2020**.

At this point, `"dev": "vite",` still works, but `"build": "run-p type-check \"build-only {@}\" --",` throws the following error:

```
src/components/HelloWorld.vue:7:20 - error TS2550: Property 'replaceAll' does not exist on type '"Hello World"'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2021' or later.

7 const test2 = test.replaceAll('o', 'a');
                     ~~~~~~~~~~


Found 2 errors.
```
Additionally, my IDE indicates that `replaceAll` is not supported with the current configuration. \
Overall, this behavior is correct and expected.

## The Bug
Adding **ES2021** to *tsconfig.app.json* should fix the project setup and everything should work, but it does not!

## The Solution
I searched for a long time until I found the cause in the tsconfig files.

In the *tsconfig.vitest.json* **lib** is overwritten, but that's not all. \
I also had to change the order of include and exclude in all tsconfig files. \
These two entries had to come after **compilerOptions**.

With these changes, everything finally works in the project, development, test and build process.
