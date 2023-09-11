# How to make a flex container overflowing horizontally?

```html
<div class="flex snap-both snap-mandatory flex-row items-center overflow-auto bg-sky-200 py-20 text-center">
  <div id="1" class="w-screen shrink-0 snap-start bg-sky-400">Who are you?</div>
  <div id="2" class="w-screen shrink-0 snap-start bg-sky-400">Where did you work before?</div>
</div>
```
You will need to add the classes `flex flex-row` to the flex container to display its flex items along the x axis.
The default behavior of flex items is to shrink to fit within their flex container. 
To prevent this, ensure that they take the full width by not shrinking using [shrink-0](https://tailwindcss.com/docs/flex-shrink#dont-shrink)
Then, set a fixed width to the flex items (in the example above, `w-screen` so as to take the full screen width).

Sandbox [here](https://play.tailwindcss.com/GxSVxiSspM?layout=horizontal).