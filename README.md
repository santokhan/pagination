# 📘 Pagination System Documentation

This module includes a reusable `Pagination` UI component and a `usePage` hook for managing pagination state via URL query parameters using `react-router`. A typical use case is fetching paginated API data.

---

## 🧩 `usePage` Hook

### 📍 Location

`src/hooks/usePage.js`

### ✅ Description

Extracts the current page, computes limit and offset from the URL query string (`?page=x`), and returns values needed for API requests.

### 📦 Code

```js
import { useSearchParams } from "react-router";

const usePage = (props = { limit: 0 }) => {
    const [URLSearchParams] = useSearchParams();
    const page = Number(URLSearchParams.get("page")) || 1;
    const limit = props?.limit || 2;
    const offset = (page - 1) * limit;

    return {
        page,
        limit,
        offset,
    };
};

export default usePage;
```

---

## 🧮 `Pagination` Component

### 📍 Location

`src/components/shared/Pagination.jsx`

### ✅ Description

Displays pagination controls (`Prev`, `Next`, and page number input`). Automatically updates the `?page=`query string using`react-router\`.

### 📦 Code

```jsx
import { Link, useSearchParams } from "react-router";
import usePage from "../../hooks/usePage";

const Pagination = ({ count = 0 }) => {
    const { page, limit } = usePage();
    const [URLSearchParams, setURLSearchParams] = useSearchParams();
    const pages = Math.ceil(count / limit);

    return (
        <div className="flex justify-between gap-4 mt-4">
            <div className="hidden lg:block grow"></div>
            <div className="flex gap-2">
                {page > 1 && (
                    <Link to={`?page=${page - 1}`} className="btn primary">
                        Prev
                    </Link>
                )}
                {page < pages && (
                    <Link to={`?page=${page + 1}`} className="btn primary">
                        Next
                    </Link>
                )}
            </div>
            <div className="grow flex gap-2 items-center justify-end">
                Page
                <input
                    type="number"
                    min={1}
                    max={pages}
                    className="bg-gray-100 w-20 p-2 rounded-lg"
                    value={page}
                    onChange={(e) => {
                        const value = e.target.value;
                        if (value > 0) {
                            URLSearchParams.set("page", value);
                            setURLSearchParams(URLSearchParams);
                        }
                    }}
                />
                of {pages}
            </div>
        </div>
    );
};

export default Pagination;
```

---

## 💡 Usage Example

### ✅ Page Component

Displays a list of contacts fetched using a custom `useContacts` hook with pagination support.

### 📦 Code

```jsx
import useContacts from "../../hooks/useContacts";
import Pagination from "../../components/shared/Pagination";
import usePage from "../../hooks/usePage";

const Page = () => {
  const { offset, limit } = usePage();
  const contacts = useContacts({ offset, limit });

  const rows = Array.isArray(contacts?.rows) ? contacts?.rows : [];
  const count = contacts?.count || 0;

  return (
    <>
      <h4 className="font-semibold text-lg">Contacts</h4>

      <div className="flex mt-4 gap-4 flex-wrap items-start">
        {rows.map((item, i) => (
          item && (
            <div
              className="border px-4 py-2.5 rounded-xl basis-80 grow max-w-md shrink-0"
              key={i}
            >
              <h5 className="font-bold">{item.name}</h5>
              <div className="mt-2">
                <a
                  href={`tel:+${(item?.phone || "").replace(/[^a-zA-Z0-9]/g, "")}`}
                  className="hover:underline"
                >
                  {item.phone}
                </a>
              </div>
              <p className="whitespace-pre-wrap mt-2">{item.message}</p>
              <div className="flex justify-end mt-2">
                <span className="text-sm">
                  {new Date(item.created_at || "").toLocaleString()}
                </span>
              </div>
            </div>
          )
        ))}
      </div>

      <Pagination count={count} />
    </>
  );
};

export default Page;
```

---

## 🧠 How It Works

1. `usePage()` extracts `page`, `limit`, and `offset` from the query string.
2. `useContacts()` fetches paginated data using `offset` and `limit`.
3. The `Pagination` component calculates the total number of pages from `count`.
4. It renders:

   * `Prev`/`Next` navigation buttons
   * A direct page number input box
5. URL updates on navigation, enabling back/forward browser support.

---

## 🧪 Test Cases

| Scenario               | Behavior                           |
| ---------------------- | ---------------------------------- |
| First page             | `Prev` is hidden                   |
| Last page              | `Next` is hidden                   |
| Middle pages           | Both `Prev` and `Next` visible     |
| Invalid `?page=` value | Falls back to `1`                  |
| Input value changed    | URL updates and triggers re-render |

---

## 🧱 Tailwind Utility Classes

Component is styled using Tailwind CSS:

* Responsive layout using Flexbox
* `btn primary` assumed to be custom classes
* Inputs and layout have rounded corners and spacing

---

Let me know if you’d like:

* TypeScript definitions
* Integration with server-side pagination
* URL synchronization improvements (debounce or custom navigation)
* A dropdown for choosing `limit` values per page
