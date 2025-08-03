// Aurien Aromas - Novo Site com Visualização de Produto, Carrinho, Filtros e Integração WhatsApp

"use client";

import { useState } from "react"; import Image from "next/image"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card"; import { ShoppingCart, Eye, Instagram } from "lucide-react";

const perfumes = [ { nome: "Miss Dior", preco: 170, tipo: "perfume", imagem: "/produtos/missdior.jpg", }, { nome: "Olympea", preco: 170, tipo: "perfume", imagem: "/produtos/olympea.jpg", }, { nome: "212 VIP", preco: 170, tipo: "perfume", imagem: "/produtos/212vip.jpg", }, { nome: "Invictus 1", preco: 170, tipo: "perfume", imagem: "/produtos/invictus1.jpg", }, { nome: "Invictus 2", preco: 170, tipo: "perfume", imagem: "/produtos/invictus2.jpg", }, { nome: "Good Girl", preco: 170, tipo: "perfume", imagem: "/produtos/goodgirl.jpg", }, { nome: "La Vie Est Belle", preco: 170, tipo: "perfume", imagem: "/produtos/lavie.jpg", }, { nome: "Asad", preco: 170, tipo: "perfume", imagem: "/produtos/asad.jpg", }, { nome: "Perfume 9", preco: 170, tipo: "perfume", imagem: "/produtos/perfume9.jpg", }, { nome: "Perfume 10", preco: 170, tipo: "perfume", imagem: "/produtos/perfume10.jpg", }, { nome: "Body Splash 1", preco: 70, tipo: "body splash", imagem: "/produtos/bodysplash1.jpg", }, { nome: "Body Splash 2", preco: 70, tipo: "body splash", imagem: "/produtos/bodysplash2.jpg", }, { nome: "Body Splash 3", preco: 70, tipo: "body splash", imagem: "/produtos/bodysplash3.jpg", }, { nome: "Body Splash 4", preco: 70, tipo: "body splash", imagem: "/produtos/bodysplash4.jpg", }, { nome: "Body Splash 5", preco: 70, tipo: "body splash", imagem: "/produtos/bodysplash5.jpg", }, { nome: "Perfume de Cabelo 1", preco: 60, tipo: "cabelo", imagem: "/produtos/cabelo1.jpg", }, { nome: "Perfume de Cabelo 2", preco: 60, tipo: "cabelo", imagem: "/produtos/cabelo2.jpg", }, { nome: "Perfume de Cabelo 3", preco: 60, tipo: "cabelo", imagem: "/produtos/cabelo3.jpg", }, ];

export default function Home() { const [filtro, setFiltro] = useState("todos"); const [visualizar, setVisualizar] = useState(null);

const produtosFiltrados = filtro === "todos" ? perfumes : perfumes.filter((p) => p.tipo === filtro);

return ( <div className="p-4 max-w-screen-xl mx-auto"> <header className="text-center mb-8"> <h1 className="text-4xl font-bold">Aurien Aromas</h1> <p className="text-lg mt-2 text-muted-foreground"> Perfumes inspirados nas melhores fragrâncias do mundo. Qualidade, presença e elegância em cada borrifada. </p> <a
href="https://www.instagram.com/aurienaromas"
target="_blank"
rel="noopener noreferrer"
className="inline-flex items-center mt-4 text-purple-600 hover:underline"
> <Instagram className="w-4 h-4 mr-2" /> @aurienaromas </a> </header>

<div className="flex justify-center gap-4 mb-6">
    <Button onClick={() => setFiltro("todos")}>Todos</Button>
    <Button onClick={() => setFiltro("perfume")}>Perfumes</Button>
    <Button onClick={() => setFiltro("body splash")}>Body Splash</Button>
    <Button onClick={() => setFiltro("cabelo")}>Perfume de Cabelo</Button>
  </div>

  <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
    {produtosFiltrados.map((produto, index) => (
      <Card key={index} className="relative group">
        <Image
          src={produto.imagem}
          alt={produto.nome}
          width={300}
          height={300}
          className="rounded-t-xl object-cover h-60 w-full"
        />
        <CardContent className="p-4 text-center">
          <h2 className="text-lg font-bold">{produto.nome}</h2>
          <p className="text-muted-foreground">R$ {produto.preco.toFixed(2)}</p>
          <div className="flex justify-center gap-2 mt-2">
            <Button size="sm" variant="outline" onClick={() => setVisualizar(produto)}>
              <Eye className="w-4 h-4 mr-1" /> Visualizar
            </Button>
            <Button size="sm">
              <ShoppingCart className="w-4 h-4 mr-1" /> Comprar
            </Button>
          </div>
        </CardContent>
      </Card>
    ))}
  </div>

  {visualizar && (
    <div className="fixed inset-0 bg-black/70 z-50 flex items-center justify-center">
      <div className="bg-white rounded-xl p-6 w-[90%] max-w-md relative">
        <button
          className="absolute top-2 right-2 text-gray-600 hover:text-black"
          onClick={() => setVisualizar(null)}
        >
          ✕
        </button>
        <Image
          src={visualizar.imagem}
          alt={visualizar.nome}
          width={400}
          height={400}
          className="rounded-xl object-cover w-full h-64"
        />
        <h2 className="text-2xl font-bold mt-4">{visualizar.nome}</h2>
        <p className="text-muted-foreground mb-4">
          R$ {visualizar.preco.toFixed(2)}
        </p>
        <p className="text-sm text-gray-500">
          Fragrância com ótima fixação, inspirada em perfumes de grife. Leve, marcante e com presença.
        </p>
        <Button className="mt-4 w-full">
          <ShoppingCart className="w-4 h-4 mr-1" /> Comprar
        </Button>
      </div>
    </div>
  )}
</div>

); }

