import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button";

export default function LifeHelperAI() { const [messages, setMessages] = useState([ { sender: "ai", text: "Hello! Main tumhara Life Helper AI hoon. Batao, kya tension hai? Main madad karne ke liye hoon!" }, ]); const [input, setInput] = useState("");

const handleSend = async () => { if (!input.trim()) return;

const userMessage = { sender: "user", text: input };
setMessages([...messages, userMessage]);

const prompt = `Tum ek friendly aur motivating AI ho jo logon ki problems sunta hai aur unhe positive aur practical advice deta hai. Ye baat hai: "${input}". Iska ek pyaara, positive jawab do.`;

const response = await fetch("https://api.openai.com/v1/chat/completions", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer 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`,
  },
  body: JSON.stringify({
    model: "gpt-3.5-turbo",
    messages: [
      { role: "system", content: "Tum ek pyaare, dost jaise AI ho jo motivate karta hai." },
      { role: "user", content: prompt },
    ],
    max_tokens: 150,
    temperature: 0.8,
  }),
});

const data = await response.json();
const aiMessage = { sender: "ai", text: data.choices[0]?.message?.content || "Kuch galat ho gaya, dobara try karo!" };

setMessages((prev) => [...prev, aiMessage]);
setInput("");

};

return ( <div className="max-w-lg mx-auto p-4 space-y-4"> <Card> <CardContent className="space-y-4 max-h-[500px] overflow-y-auto"> {messages.map((msg, index) => ( <div key={index} className={p-3 rounded-2xl ${msg.sender === "ai" ? "bg-blue-100 text-blue-800" : "bg-green-100 text-green-800"}} > {msg.text} </div> ))} </CardContent> </Card>

<div className="flex items-center space-x-2">
    <Input
      placeholder="Apni baat yahan likho..."
      value={input}
      onChange={(e) => setInput(e.target.value)}
      onKeyDown={(e) => e.key === "Enter" && handleSend()}
    />
    <Button onClick={handleSend}>Bhejo</Button>
  </div>
</div>

); }

