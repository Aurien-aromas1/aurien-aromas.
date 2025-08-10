// Site completo para Aurien Aromas com carrinho, contato e layout elegante import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { ShoppingCart, Instagram } from "lucide-react";

const products = [ { id: 1, name: "Miss Dior", type: "Perfume", price: 170, quantity: 1 }, { id: 2, name: "Good Girl", type: "Perfume", price: 170, quantity: 1 }, { id: 3, name: "Olympea", type: "Perfume", price: 170, quantity: 1 }, { id: 4, name: "212 VIP", type: "Perfume", price: 170, quantity: 1 }, { id: 5, name: "Invikitus", type: "Perfume", price: 170, quantity: 2 }, { id: 6, name: "Asad", type: "Perfume", price: 170, quantity: 1 }, { id: 7, name: "La Vie Est Belle", type: "Perfume", price: 170, quantity: 1 }, { id: 8, name: "Outro Perfume 1", type: "Perfume", price: 170, quantity: 1 }, { id: 9, name: "Outro Perfume 2", type: "Perfume", price: 170, quantity: 1 }, { id: 10, name: "Body Splash Glitter - Scandal", type: "Body Splash", price: 70, quantity: 5 }, { id: 11, name: "Perfume de Cabelo", type: "Hair Perfume", price: 50, quantity: 3 }, ];

export default function Home() { const [cart, setCart] = useState([]);

const addToCart = (product) => { setCart((prev) => { const itemExists = prev.find((item) => item.id === product.id); if (itemExists) { return prev.map((item) => item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item ); } return [...prev, { ...product, quantity: 1 }]; }); };

const total = cart.reduce((acc, item) => acc + item.price * item.quantity, 0);

return ( <div className="bg-white text-gold-900 min-h-screen p-6"> <header className="flex justify-between items-center mb-6"> <h1 className="text-3xl font-bold text-gold-800">Aurien Aromas</h1> <a href="https://www.instagram.com/aurienaromas" target="_blank" className="flex gap-2 items-center text-gold-700"> <Instagram /> @aurienaromas </a> </header>

<section className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
    {products.map((product) => (
      <Card key={product.id} className="border-gold-300 shadow-lg">
        <CardContent className="p-4">
          <h2 className="text-xl font-semibold mb-2">{product.name}</h2>
          <p className="text-sm text-gray-600 mb-1">{product.type}</p>
          <p className="font-bold text-lg text-gold-800">R$ {product.price.toFixed(2)}</p>
          <Button onClick={() => addToCart(product)} className="mt-2 w-full bg-gold-600 text-white hover:bg-gold-700">
            Adicionar ao carrinho
          </Button>
        </CardContent>
      </Card>
    ))}
  </section>

  <section className="mt-10 bg-gold-50 p-4 rounded-xl shadow-md">
    <h2 className="text-2xl font-bold mb-4 flex items-center gap-2">
      <ShoppingCart /> Seu Carrinho
    </h2>
    {cart.length === 0 ? (
      <p className="text-gray-600">Seu carrinho está vazio.</p>
    ) : (
      <ul className="mb-4">
        {cart.map((item) => (
          <li key={item.id} className="flex justify-between py-1 border-b border-gray-300">
            <span>{item.name} x{item.quantity}</span>
            <span>R$ {(item.price * item.quantity).toFixed(2)}</span>
          </li>
        ))}
      </ul>
    )}
    <p className="text-xl font-semibold text-right mb-4">Total: R$ {total.toFixed(2)}</p>
    <a
      href={`https://wa.me/5545999313588?text=Olá! Quero finalizar a compra dos seguintes produtos: ${cart
        .map((item) => `${item.name} x${item.quantity}`)
        .join(", ")}. Total: R$ ${total.toFixed(2)}`}
      target="_blank"
    >
      <Button className="w-full bg-green-600 hover:bg-green-700 text-white text-lg">
        Finalizar pelo WhatsApp
      </Button>
    </a>
  </section>
</div>

); }

// Aurien Aromas - Novo Site com Visualização de Produto, Carrinho, Filtros e Integração WhatsApp

"use client";

import { useState } from "react"; import Image from "next/image"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card"; import { ShoppingCart, Eye, Instagram } from "lucide-react";

const perfumes = [ { nome: "Miss Dior", preco: 170, tipo: "perfume", imagem: "/produtos/missdior.jpg", }, { nome: "Olympea", preco: 170, tipo: "perfume", imagem: "/produtos/olympea.jpg", }, { nome: "212 VIP", preco: 170, tipo: "perfume", imagem: "/produtos/212vip.jpg", }, { nome: "Invictus 1", preco: 170, tipo: "perfume", imagem: "/produtos/invictus1.jpg", }, { nome: "Invictus 2", preco: 170, tipo: "perfume", imagem: "/produtos/invictus2.jpg", }, { nome: "Good Girl", preco: 170, tipo: "perfume", imagem: "/produtos/goodgirl.jpg", }, { nome: "La Vie Est Belle", preco: 170, tipo: "perfume", imagem: "/produtos/lavie.jpg", }, { nome: "Asad", preco: 170, tipo: "perfume", imagem: "/produtos/asad.jpg", }, { nome: "Perfume 9", preco: 170, tipo: "perfume", imagem: "/produtos/perfume9.jpg", }, { nome: "Perfume 10", preco: 
