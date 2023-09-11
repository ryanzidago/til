# How to remove the scrollbar?

Place the code below in the `global.css` file:
```scss
//global index.css
@tailwind base;
@tailwind components;
@tailwind utilities;

// add the code bellow
@layer utilities {
    @variants responsive {
      /* Hide scrollbar for Chrome, Safari and Opera */
      .no-scrollbar::-webkit-scrollbar {
          display: none;
      }

      /* Hide scrollbar for IE, Edge and Firefox */
      .no-scrollbar {
          -ms-overflow-style: none;  /* IE and Edge */
          scrollbar-width: none;  /* Firefox */
      }
    }
  }
```

Add the `no-scrollbar` class to your desired element.

Source [here](https://dev.to/derick1530/how-to-create-scrollable-element-in-tailwind-without-a-scrollbar-4mbd).