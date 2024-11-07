To implement pagination like the one you described (`< 1 ... 8 9 10 11 12 ... 100 >`), you need to display the current page, some neighboring pages, and links to the first and last pages, with ellipses (`...`) to indicate skipped pages in between.

Here's how you can structure the pagination:

### Key points:
- **First and Last Page:** Always display a link to the first (`1`) and last page (`100`).
- **Current Page:** The current page should be highlighted and not be a clickable link.
- **Neighboring Pages:** Show a small range of pages before and after the current page (e.g., 4 pages before and after).
- **Ellipses (`...`):** Use ellipses to represent skipped pages if there are too many pages to display.

### Example Logic:

Let's assume you have:
- `totalPages`: Total number of pages (e.g., `100`).
- `currentPage`: The current page the user is on (e.g., `10`).

You need to decide the pages to show based on the `currentPage` and `totalPages`.

#### Pseudocode for Pagination:

```js
function generatePagination(totalPages, currentPage) {
    const pagination = [];

    // Always show the first page
    pagination.push(1);

    // Add ellipsis if there's a gap between the first page and the current page
    if (currentPage > 4) {
        pagination.push('...');
    }

    // Show a range of pages around the current page
    const start = Math.max(2, currentPage - 4);
    const end = Math.min(totalPages - 1, currentPage + 4);

    for (let i = start; i <= end; i++) {
        pagination.push(i);
    }

    // Add ellipsis if there's a gap between the current page range and the last page
    if (currentPage < totalPages - 4) {
        pagination.push('...');
    }

    // Always show the last page
    if (totalPages > 1) {
        pagination.push(totalPages);
    }

    return pagination;
}
```

#### Example Output:

1. For `totalPages = 100`, `currentPage = 10`:

```js
[1, '...', 6, 7, 8, 9, 10, 11, 12, '...', 100]
```

2. For `totalPages = 100`, `currentPage = 50`:

```js
[1, '...', 46, 47, 48, 49, 50, 51, 52, 53, '...', 100]
```

3. For `totalPages = 100`, `currentPage = 95`:

```js
[1, '...', 91, 92, 93, 94, 95, 96, 97, 98, 99, 100]
```

### HTML Structure

This is a basic HTML structure that you can use to render the pagination:

```html
<div class="pagination">
    <a href="#">&lt;</a> <!-- Previous button -->
    <a href="#">1</a> <!-- First page -->
    <span>...</span> <!-- Ellipses -->
    <a href="#">6</a> <!-- Page number -->
    <a href="#">7</a> <!-- Page number -->
    <a href="#">8</a> <!-- Page number -->
    <a href="#">9</a> <!-- Page number -->
    <a href="#">10</a> <!-- Current page (not clickable, just a span) -->
    <a href="#">11</a> <!-- Page number -->
    <a href="#">12</a> <!-- Page number -->
    <span>...</span> <!-- Ellipses -->
    <a href="#">100</a> <!-- Last page -->
    <a href="#"> &gt;</a> <!-- Next button -->
</div>
```

### CSS Styling (Optional)

You can style the pagination with CSS for better presentation:

```css
.pagination a {
    padding: 8px 12px;
    margin: 0 5px;
    text-decoration: none;
    color: #007bff;
    border: 1px solid #ddd;
    border-radius: 3px;
}

.pagination a:hover {
    background-color: #f0f0f0;
}

.pagination .current {
    background-color: #007bff;
    color: white;
    cursor: default;
}

.pagination span {
    padding: 8px 12px;
    margin: 0 5px;
    color: #555;
}
```

This structure should give you a pagination that looks like:

```
< 1 ... 8 9 10 11 12 ... 100 >
```

By adjusting the `currentPage` and `totalPages` values dynamically, this will handle most pagination scenarios.
