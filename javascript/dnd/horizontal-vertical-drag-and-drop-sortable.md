# Horziontal vertical drag and drop sortable with dnd, using NextJS, Typescript and TailwindCSS

Given a row of columns where each column contains many rows (where each row represents an item); I wanted the user to be able to:
- dragging and dropping a column betwen other columns to re-order them
- dragging and dropping a row within a colum to re-order the column's rows
- dragging and dropping a column row into another column (moving an item from a column into another one)

Exactly like a project management software where tickets are dragged and dropped across statuses. 

To do this I needed to first:
- Create a `SortableContext` with a `horizontalListSortingStrategy` that will contain each columns and allow them to be re-ordered by way of drag and drop
- For each column, create a `SortableContext` with a `verticalListSortingStrategy` so that rows within a column can be re-ordered by way of drag and drop
- Create handler functions to deal with a row as it is being dragged from one column into another one

Live example available [here](https://codesandbox.io/p/sandbox/horizontal-vertical-drag-and-drop-sortable-xp8q49?file=%2Fapp%2Fpage.tsx%3A14%2C17).
