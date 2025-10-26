# Digital-bazar
online shopping

import React, { useState } from "react";

// Digital Bazaar — A single-file React component (Tailwind) that mimics a simple Daraz-like storefront.
// How to use: Drop this component into a React app (Vite / Create React App). Tailwind must be configured.
// This file is intentionally self-contained for demo and prototyping purposes.

export default function DigitalBazaar() {
  const [query, setQuery] = useState("");
  const [category, setCategory] = useState("All");
  const [cart, setCart] = useState([]);
  const [selected, setSelected] = useState(null);

  const PRODUCTS = [
    { id: 1, name: "Smartphone X1", price: 37999, cat: "Mobiles", img: "https://via.placeholder.com/400x300?text=Phone+X1" },
    { id: 2, name: "Laptop Pro 14'", price: 129999, cat: "Computers", img: "https://via.placeholder.com/400x300?text=Laptop+Pro" },
    { id: 3, name: "Men's Sneakers", price: 4999, cat: "Fashion", img: "https://via.placeholder.com/400x300?text=Sneakers" },
    { id: 4, name: "LED TV 43''", price: 62999, cat: "Electronics", img: "https://via.placeholder.com/400x300?text=LED+TV" },
    { id: 5, name: "Blender 2L", price: 3499, cat: "Home & Kitchen", img: "https://via.placeholder.com/400x300?text=Blender" },
  ];

  const categories = ["All", "Mobiles", "Computers", "Fashion", "Electronics", "Home & Kitchen"];

  function addToCart(product) {
    setCart((c) => {
      const exists = c.find((x) => x.id === product.id);
      if (exists) return c.map((x) => (x.id === product.id ? { ...x, qty: x.qty + 1 } : x));
      return [...c, { ...product, qty: 1 }];
    });
  }

  function removeFromCart(productId) {
    setCart((c) => c.filter((x) => x.id !== productId));
  }

  function changeQty(productId, delta) {
    setCart((c) =>
      c
        .map((x) => (x.id === productId ? { ...x, qty: Math.max(1, x.qty + delta) } : x))
        .filter(Boolean)
    );
  }

  const filtered = PRODUCTS.filter((p) => {
    if (category !== "All" && p.cat !== category) return false;
    if (!query) return true;
    return p.name.toLowerCase().includes(query.toLowerCase());
  });

  const total = cart.reduce((s, it) => s + it.price * it.qty, 0);

  return (
    <div className="min-h-screen bg-gray-50 text-gray-800">
      {/* Header */}
      <header className="bg-white shadow-md sticky top-0 z-20">
        <div className="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-md bg-gradient-to-br from-purple-600 to-pink-500 flex items-center justify-center text-white font-bold">
              DB
            </div>
            <div>
              <h1 className="font-extrabold text-lg">Digital Bazaar</h1>
              <p className="text-xs text-gray-500">اپنا دراز اسٹائل مارکیٹ</p>
            </div>
          </div>

          <div className="flex-1 mx-6">
            <div className="flex bg-gray-100 rounded-md overflow-hidden">
              <input
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Search products, brands or categories..."
                className="w-full px-4 py-2 outline-none bg-transparent"
              />
              <button className="px-4 py-2 font-semibold">Search</button>
            </div>
          </div>

          <div className="flex items-center gap-4">
            <button className="text-sm">Login</button>
            <button className="text-sm">Seller Center</button>
            <div className="relative">
              <button
                onClick={() => setSelected({ type: "cart" })}
                className="px-3 py-2 bg-pink-600 text-white rounded-md font-semibold"
              >
                Cart ({cart.length})
              </button>
            </div>
          </div>
        </div>
      </header>

      {/* Main */}
      <main className="max-w-6xl mx-auto px-4 py-8 grid grid-cols-1 md:grid-cols-4 gap-6">
        {/* Sidebar */}
        <aside className="md:col-span-1 bg-white rounded-md p-4 shadow-sm">
          <h3 className="font-semibold mb-3">Categories</h3>
          <ul className="space-y-2">
            {categories.map((c) => (
              <li key={c}>
                <button
                  onClick={() => setCategory(c)}
                  className={`w-full text-left px-3 py-2 rounded-md ${c === category ? "bg-pink-50 font-medium" : "hover:bg-gray-50"}`}
                >
                  {c}
                </button>
              </li>
            ))}
          </ul>

          <div className="mt-6">
            <h4 className="font-semibold">Delivery to</h4>
            <p className="text-sm text-gray-600">Lahore, Punjab</p>
          </div>
        </aside>

        {/* Products */}
        <section className="md:col-span-3">
          <div className="bg-white p-4 rounded-md shadow-sm mb-4 flex items-center justify-between">
            <div>
              <h2 className="font-bold">Trending Products</h2>
              <p className="text-sm text-gray-600">Showing {filtered.length} results</p>
            </div>
            <div className="text-sm text-gray-600">Sort: Popular</div>
          </div>

          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
            {filtered.map((p) => (
              <div key={p.id} className="bg-white rounded-md shadow-sm overflow-hidden">
                <img src={p.img} alt={p.name} className="w-full h-48 object-cover" />
                <div className="p-3">
                  <h3 className="font-semibold truncate">{p.name}</h3>
                  <p className="mt-1 text-pink-600 font-bold">Rs {p.price.toLocaleString()}</p>
                  <p className="text-xs text-gray-500 mt-1">Category: {p.cat}</p>
                  <div className="mt-3 flex gap-2">
                    <button onClick={() => addToCart(p)} className="flex-1 px-3 py-2 bg-pink-600 text-white rounded-md">Add to Cart</button>
                    <button onClick={() => setSelected({ type: "detail", product: p })} className="px-3 py-2 border rounded-md">View</button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </section>
      </main>

      {/* Footer */}
      <footer className="bg-white border-t mt-8">
        <div className="max-w-6xl mx-auto px-4 py-6 text-sm text-gray-600">© {new Date().getFullYear()} Digital Bazaar — All rights reserved.</div>
      </footer>

      {/* Drawer / Modal area */}
      {selected && selected.type === "detail" && (
        <div className="fixed inset-0 bg-black/40 flex items-center justify-center z-30">
          <div className="bg-white rounded-md w-11/12 md:w-2/3 p-4">
            <div className="flex gap-4">
              <img src={selected.product.img} alt="" className="w-48 h-48 object-cover rounded-md" />
              <div className="flex-1">
                <h3 className="font-bold text-xl">{selected.product.name}</h3>
                <p className="text-pink-600 font-bold mt-2">Rs {selected.product.price.toLocaleString()}</p>
                <p className="mt-3 text-sm text-gray-700">یہ ایک مثال پروڈکٹ ڈسکرپشن ہے — آپ یہاں مکمل تفصیل، خصوصیات اور شیئیرنگ آپشنز رکھ سکتے ہیں۔</p>

                <div className="mt-4 flex gap-2">
                  <button onClick={() => addToCart(selected.product)} className="px-4 py-2 bg-pink-600 text-white rounded-md">Add to Cart</button>
                  <button onClick={() => setSelected(null)} className="px-4 py-2 border rounded-md">Close</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      )}

      {selected && selected.type === "cart" && (
        <div className="fixed right-0 top-0 h-full w-full md:w-96 bg-white shadow-lg z-40">
          <div className="p-4 flex items-center justify-between border-b">
            <h3 className="font-bold">Your Cart</h3>
            <button onClick={() => setSelected(null)} className="text-sm text-gray-600">Close</button>
          </div>
          <div className="p-4 space-y-4 overflow-y-auto h-[70vh]">
            {cart.length === 0 && <div className="text-gray-500">Cart is empty.</div>}
            {cart.map((it) => (
              <div key={it.id} className="flex items-center gap-3">
                <img src={it.img} className="w-16 h-16 object-cover rounded-md" />
                <div className="flex-1">
                  <div className="font-semibold">{it.name}</div>
                  <div className="text-sm text-gray-600">Rs {it.price.toLocaleString()}</div>
                  <div className="mt-2 flex items-center gap-2">
                    <button onClick={() => changeQty(it.id, -1)} className="px-2 py-1 border rounded">-</button>
                    <div className="px-3">{it.qty}</div>
                    <button onClick={() => changeQty(it.id, 1)} className="px-2 py-1 border rounded">+</button>
                    <button onClick={() => removeFromCart(it.id)} className="ml-3 text-sm text-red-600">Remove</button>
                  </div>
                </div>
              </div>
            ))}
          </div>

          <div className="p-4 border-t">
            <div className="flex items-center justify-between font-semibold"> <span>Total</span> <span>Rs {total.toLocaleString()}</span></div>
            <div className="mt-3 grid grid-cols-2 gap-2">
              <button className="px-3 py-2 border rounded">Continue Shopping</button>
              <button className="px-3 py-2 bg-pink-600 text-white rounded">Proceed to Checkout</button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

/*
Customization & Deployment Notes (add to your project's README):

1) TailwindCSS
 - Install Tailwind and configure according to your React setup (Vite / CRA).
 - This component uses Tailwind utility classes. If Tailwind is not present, styles will not work.

2) Images
 - Replace placeholder image URLs with real product images (CDN or storage).

3) Authentication & Backend
 - For production, connect to a backend API for products, cart persistence, user auth and payments.
 - You can use Firebase, Supabase, or a Node.js + PostgreSQL backend.

4) Seller features
 - Add routes/pages for Seller Dashboard, product upload form, order management.

5) Mobile-friendly
 - The layout uses responsive Tailwind grid. Test across devices and tweak breakpoints.

6) Branding
 - Replace the "DB" logo block and site title with your real logo and brand colors.

7) PWA & Play Store
 - To publish as an Android webapp, wrap with Trusted Web Activity or build with Capacitor.

8) Recommended next steps I can do for you (choose one):
 - Convert this single-file demo into a multi-page React app (Routing + pages)
 - Add Firebase authentication + Firestore product backend
 - Create a Seller onboarding flow
 - Generate a logo and color palette for Digital Bazaar
*/
