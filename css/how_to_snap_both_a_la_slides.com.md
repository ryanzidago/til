# How to snap-both à la slides.com?

```html
<div class="no-scrollbar flex h-screen w-screen snap-x snap-mandatory flex-row overflow-auto">
  <div class="no-scrollbar flex w-screen shrink-0 snap-y snap-mandatory snap-start flex-col overflow-auto bg-sky-500">
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hello, world!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Bonjour, monde!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hallo, Welt!</div>
  </div>
  <div class="no-scrollbar flex w-screen shrink-0 snap-y snap-mandatory snap-start flex-col overflow-auto bg-emerald-500">
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hello, world!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Bonjour, monde!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hallo, Welt!</div>
  </div>
  <div class="no-scrollbar flex w-screen shrink-0 snap-y snap-mandatory snap-start flex-col overflow-auto bg-yellow-500">
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hello, world!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Bonjour, monde!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hallo, Welt!</div>
  </div>
  <div class="no-scrollbar flex w-screen shrink-0 snap-y snap-mandatory snap-start flex-col overflow-auto bg-pink-500">
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hello, world!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Bonjour, monde!</div>
    <div class="flex h-screen w-full shrink-0 snap-start items-center justify-center">Hallo, Welt!</div>
  </div>
</div>
```

Sandbox [here](https://play.tailwindcss.com/N47ru0VKKj).
