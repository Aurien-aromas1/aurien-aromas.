# aurien-aromas.
import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { ShoppingCart, Instagram } from "lucide-react";

const categories = ["Todos", "Perfume", "Body Splash", "Cabelo"];

export default function HomePage() {
  const [selectedCategory, setSelectedCategory] = useState("Todos");

  const filteredProducts =
    selectedCategory === "Todos"
      ? products
      : products.filter((p) => p.type === selectedCategory);

  return (
    <main className="min-h-screen bg-gradient-to-b from-zinc-100 to-white text-zinc-900">
      <section className="text-center py-10 px-4">
        <h1 className="text-4xl font-bold mb-2">Aurien</h1>
        <p className="text-lg text-zinc-600">
          Perfumes e aromas que marcam presença
        </p>
      </section>

      {/* Filtro de categorias */}
      <section className="flex justify-center gap-4 flex-wrap px-4 mb-4">
        {categories.map((cat) => (
          <Button
            key={cat}
            variant={selectedCategory === cat ? "default" : "outline"}
            onClick={() => setSelectedCategory(cat)}
          >
            {cat}
          </Button>
        ))}
      </section>

      <section className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6 px-6 py-10">
        {filteredProducts.map((product, i) => (
          <Card key={i} className="rounded-2xl shadow-lg">
            <img
              src={`https://source.unsplash.com/400x400/?${product.img}`}
              alt={product.name}
              className="rounded-t-2xl w-full h-64 object-cover"
            />
            <CardContent className="p-4">
              <h3 className="text-xl font-semibold">{product.name}</h3>
              <p className="text-zinc-600 mt-1 mb-1">{product.type}</p>
              <p className="text-sm text-zinc-500 mb-3">{product.desc}</p>
              <div className="flex justify-between items-center">
                <span className="text-lg font-bold">
                  R$ {product.price.toFixed(2)}
                </span>
                <Button>
                  <ShoppingCart className="mr-2 w-4 h-4" /> Comprar
                </Button>
              </div>
            </CardContent>
          </Card>
        ))}
      </section>

      {/* Instagram */}
      <section className="text-center py-6">
        <a
          href="https://www.instagram.com/aurienaromas?igsh=MTY1am5qc21zeHRhMw=="
          target="_blank"
          rel="noopener noreferrer"
          className="inline-flex items-center gap-2 px-4 py-2 text-white bg-pink-600 hover:bg-pink-700 rounded-full text-sm"
        >
          <Instagram className="w-4 h-4" /> Siga @aurienaromas no Instagram
        </a>
      </section>

      {/* Rodapé */}
      <footer className="text-center py-6 text-sm text-zinc-500">
        <p>© 2025 Aurien. Todos os direitos reservados.</p>
        <div className="flex justify-center gap-4 mt-2 text-xs">
          <a href="#" className="hover:underline">Sobre</a>
          <a href="#" className="hover:underline">Contato</a>
          <a href="#" className="hover:underline">Política de Privacidade</a>
        </div>
      </footer>
    </main>
  );
} import { Button } from "@/components/ui/button";

interface CategoryFilterProps {
  categories: string[];
  selectedCategory: string;
  onSelect: (category: string) => void;
}

export default function CategoryFilter({
  categories,
  selectedCategory,
  onSelect,
}: CategoryFilterProps) {
  return (
    <div className="flex justify-center gap-4 flex-wrap px-4 mb-4">
      {categories.map((cat) => (
        <Button
          key={cat}
          variant={selectedCategory === cat ? "default" : "outline"}
          onClick={() => onSelect(cat)}
        >
          {cat}
        </Button>
      ))}
    </div>
  );
}import { createContext, useContext, useState, ReactNode } from "react";

interface Product {
  name: string;
  price: number;
}

interface CartContextType {
  cart: Product[];
  addToCart: (product: Product) => void;
  clearCart: () => void;
}

const CartContext = createContext<CartContextType | undefined>(undefined);

export function CartProvider({ children }: { children: ReactNode }) {
  const [cart, setCart] = useState<Product[]>([]);

  const addToCart = (product: Product) => setCart([...cart, product]);
  const clearCart = () => setCart([]);

  return (
    <CartContext.Provider value={{ cart, addToCart, clearCart }}>
      {children}
    </CartContext.Provider>
  );
}

export function useCart() {
  const context = useContext(CartContext);
  if (!context) throw new Error("useCart must be used within CartProvider");
  return context;
}import { useCart } from "@/context/CartContext";

export default function CheckoutButton() {
  const { cart } = useCart();

  const total = cart.reduce((sum, item) => sum + item.price, 0);
  const message = encodeURIComponent(
    `Olá! Quero finalizar a compra com os seguintes itens:\n\n${cart
      .map((p) => `• ${p.name} - R$ ${p.price.toFixed(2)}`)
      .join("\n")}\n\nTotal: R$ ${total.toFixed(2)}`
  );

  const whatsappUrl = `https://wa.me/SEU_NUMERO?text=${message}`; // Substitua SEU_NUMERO

  return (
    <a
      href={whatsappUrl}
      target="_blank"
      rel="noopener noreferrer"
      className="inline-block bg-green-600 text-white px-4 py-2 rounded-lg mt-4 hover:bg-green-700"
    >
      Finalizar Compra via WhatsApp
    </a>
  );
}slug: "miss-dior", // Adicione isso em cada produtoimport { useRouter } from "next/router";
import products from "@/data/products"; // adapte conforme seu projeto

export default function ProdutoPage() {
  const router = useRouter();
  const { slug } = router.query;

  const product = products.find((p) => p.slug === slug);

  if (!product) return <p>Produto não encontrado.</p>;

  return (
    <div className="max-w-xl mx-auto py-10 px-4">
      <h1 className="text-3xl font-bold">{product.name}</h1>
      <p className="text-zinc-600">{product.desc}</p>
      <img
        src={`https://source.unsplash.com/600x600/?${product.img}`}
        alt={product.name}
        className="my-6 w-full rounded-xl"
      />
      <p className="text-lg font-bold mb-4">R$ {product.price.toFixed(2)}</p>
      {/* Botão de compra ou adicionar ao carrinho aqui */}
    </div>
  );
}import { CartProvider } from "@/context/CartContext";

function MyApp({ Component, pageProps }) {
  return (
    <CartProvider>
      <Component {...pageProps} />
    </CartProvider>
  );
}

export default MyApp;export const products = [
  { name: "Miss Dior", slug: "miss-dior", type: "Perfume", price: 170.0, img: "miss-dior", desc: "Notas florais elegantes com um toque adocicado de sofisticação." },
  // ... todos os produtos, com slug único ...
];import { createContext, useContext, useState, ReactNode } from "react";
import type { Product } from "@/data/products";

interface CartContextType {
  cart: Product[];
  addToCart: (p: Product) => void;
  clearCart: () => void;
}

const CartContext = createContext<CartContextType | undefined>(undefined);

export function CartProvider({ children }: { children: ReactNode }) {
  const [cart, setCart] = useState<Product[]>([]);
  return (
    <CartContext.Provider
      value={{
        cart,
        addToCart: (p) => setCart((c) => [...c, p]),
        clearCart: () => setCart([]),
      }}
    >
      {children}
    </CartContext.Provider>
  );
}

export function useCart() {
  const ctx = useContext(CartContext);
  if (!ctx) throw new Error("useCart used outside CartProvider");
  return ctx;
}import { Button } from "@/components/ui/button";

interface Props {
  categories: string[];
  selected: string;
  onSelect: (cat: string) => void;
}

export default function CategoryFilter({ categories, selected, onSelect }: Props) {
  return (
    <div className="flex justify-center gap-4 flex-wrap px-4 mb-4">
      {categories.map((cat) => (
        <Button
          key={cat}
          variant={selected === cat ? "default" : "outline"}
          onClick={() => onSelect(cat)}
        >
          {cat}
        </Button>
      ))}
    </div>
  );
}import { useCart } from "@/context/CartContext";

export default function CheckoutButton() {
  const { cart } = useCart();
  const total = cart.reduce((sum, p) => sum + p.price, 0);
  const message = encodeURIComponent(
    `Olá! Quero finalizar a compra com os itens:\n${cart.map(p => `• ${p.name} – R$ ${p.price.toFixed(2)}`).join("\n")}\n\nTotal: R$ ${total.toFixed(2)}`
  );
  const url = `https://wa.me/5545999313588?text=${message}`;

  return (
    <a href={url} target="_blank" rel="noopener noreferrer"
      className="bg-green-600 text-white px-4 py-2 rounded-lg mt-4 hover:bg-green-700">
      Finalizar Compra via WhatsApp
    </a>
  );
}import { useState } from "react";
import { products } from "@/data/products";
import CategoryFilter from "@/components/CategoryFilter";
import { Card, CardContent } from "@/components/ui/card";
port { ShoppingCart } from "lucide-react";
import { useCart } from "@/context/CartContext";
import Link from "next/link";

const categories = ["Todos", "Perfume", "Body Splash", "Cabelo"];

export default function HomePage() {
  const [selected, setSelected] = useState("Todos");
  const { addToCart, cart } = useCart();
  const filtered = selected === "Todos" ? products : products.filter(p => p.type === selected);

  return (
    <main className="min-h-screen bg-gradient-to-b from-zinc-100 to-white text-zinc-900">
      <section className="text-center py-10 px-4">
        <h1 className="text-4xl font-bold mb-2">Aurien</h1>
        <p className="text-lg text-zinc-600">Perfumes e aromas que marcam presença</p>
      </section>

      <CategoryFilter categories={categories} selected={selected} onSelect={setSelected} />

      <section className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6 px-6 py-10">
        {filtered.map((p) => (
          <Card key={p.slug} className="rounded-2xl shadow-lg">
            <Link href={`/produto/${p.slug}`}>
              <img src={`https://source.unsplash.com/400x400/?${p.img}`}
                   alt={p.name}
                   className="rounded-t-2xl w-full h-64 object-cover" />
            </Link>
            <CardContent className="p-4">
              <Link href={`/produto/${p.slug}`}>
                <h3 className="text-xl font-semibold">{p.name}</h3>
              </Link>
              <p className="text-zinc-600 mt-1 mb-1">{p.type}</p>
              <p className="text-sm text-zinc-500 mb-3">{p.desc}</p>
              <div className="flex justify-between items-center">
                <span className="text-lg font-bold">R$ {p.price.toFixed(2)}</span>
                <button onClick={() => addToCart(p)} className="text-green-600 hover:underline">
                  <ShoppingCart className="inline-block mr-1 w-4 h-4" />Adicionar
                </button>
              </div>
            </CardContent>
          </Card>
        ))}
      </section>

      {cart.length > 0 && <div className="fixed bottom-4 right-4">
        <Link href="/carrinho">
          <button className="bg-pink-600 text-white px-4 py-2 rounded-full shadow-lg">
            Ver Carrinho ({cart.length})
          </button>
        </Link>
      </div>}
    </main>
  );
}import { useRouter } from "next/router";
import { products } from "@/data/products";
import { useCart } from "@/context/CartContext";
import CheckoutButton from "@/components/CheckoutButton";

export default function ProdutoPage() {
  const { slug } = useRouter().query;
  const product = products.find(p => p.slug === slug);
  const { addToCart } = useCart();

  if (!product) return <p>Produto não encontrado.</p>;

  return (
    <div className="max-w-xl mx-auto py-10 px-4">
      <h1 className="text-3xl font-bold">{product.name}</h1>
      <img src={`https://source.unsplash.com/600x600/?${product.img}`} alt={product.name}
           className="my-6 w-full rounded-xl" />
      <p className="text-zinc-600 mb-4">{product.desc}</p>
      <p className="text-lg font-bold mb-4">R$ {product.price.toFixed(2)}</p>
      <button onClick={() => addToCart(product)}
        className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
        Adicionar ao Carrinho
      </button>
      <CheckoutButton />
    </div>
  );
}import { AppProps } from "next/app";
import { CartProvider } from "@/context/CartContext";
import "@/styles/globals.css";

export default function MyApp({ Component, pageProps }: AppProps) {
  return (
    <CartProvider>
      <Component {...pageProps} />
    </CartProvider>
  );
}npm install tailwindcss@latest postcss@latest autoprefixer@latest
npm install lucide-react shadcn-uiimport { useCart } from "@/context/CartContext";
import CheckoutButton from "@/components/CheckoutButton";
import Link from "next/link";

export default function CarrinhoPage() {
  const { cart, clearCart, removeFromCart } = useCart();
  const total = cart.reduce((sum, item) => sum + item.price, 0);

  if (!cart.length) {
    return (
      <div className="max-w-xl mx-auto py-10 px-4">
        <h2 className="text-2xl font-bold mb-4">Seu carrinho está vazio</h2>
        <Link href="/">
          <a className="text-pink-600 hover:underline">Voltar para a loja</a>
        </Link>
      </div>
    );
  }

  return (
    <div className="max-w-2xl mx-auto py-10 px-4">
      <h2 className="text-2xl font-bold mb-6">Carrinho de Compras</h2>
      <ul className="space-y-4">
        {cart.map((item, idx) => (
          <li key={idx} className="flex justify-between items-center">
            <div>
              <p className="font-semibold">{item.name}</p>
              <p className="text-zinc-600">R$ {item.price.toFixed(2)}</p>
            </div>
            <button
              onClick={() => removeFromCart(idx)}
              className="text-red-600 hover:underline"
            >
              Remover
            </button>
          </li>
        ))}
      </ul>

      <div className="mt-6 flex justify-between items-center">
        <p className="text-lg font-bold">Total: R$ {total.toFixed(2)}</p>
        <button
          onClick={clearCart}
          className="text-sm text-zinc-500 hover:underline"
        >
          Limpar carrinho
        </button>
      </div>

      <div className="mt-8">
        <CheckoutButton />
      </div>
    </div>
  );
}// Dentro do CartContext, adicione:
const removeFromCart = (index: number) => setCart(c => c.filter((_, i) => i !== index));const pixKey = "SEU_PIX_KEY";
const message = encodeURIComponent(
  `Olá! Quero finalizar com pagamento via Pix.\nCódigo Pix: ${pixKey}\nItens:\n${cart.map(p => `• ${p.name} – R$ ${p.price.toFixed(2)}`).join("\n")}\nTotal: R$ ${total.toFixed(2)}`
);
const url = `https://wa.me/5545999313588?text=${message}`;
