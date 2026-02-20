# Storytelling Guide: Making Code Understandable for Kids

## Table of Contents
1. [Core Metaphor System](#core-metaphor-system)
2. [Story Structure](#story-structure)
3. [Language Rules](#language-rules)
4. [Metaphor Library by Component Type](#metaphor-library-by-component-type)
5. [Example Story Narratives](#example-story-narratives)

## Core Metaphor System

Map technical concepts to real-world things a 10-year-old already knows:

| Technical Concept | Kid-Friendly Metaphor (EN) | Kid-Friendly Metaphor (ZH) |
|---|---|---|
| Server | A chef in a restaurant kitchen | 餐厅厨房里的大厨 |
| Client/Frontend | The menu and dining area | 菜单和用餐区 |
| Database | A giant filing cabinet | 一个巨大的文件柜 |
| API | A waiter carrying orders back and forth | 来回传递订单的服务员 |
| Authentication | A security guard checking IDs | 保安叔叔检查你的证件 |
| Cache | A sticky note on the fridge | 贴在冰箱上的便利贴 |
| Queue | A line of people waiting for a ride | 排队等坐过山车 |
| Load Balancer | A traffic policeman directing cars | 指挥交通的警察叔叔 |
| Middleware | A checkpoint on a road | 路上的检查站 |
| Config | The settings/rules book | 规则手册 |
| Router | A signpost at a crossroads | 十字路口的路标 |
| State | What the app is "thinking" right now | 应用现在"脑子里想的事" |
| Component | A LEGO brick | 一块乐高积木 |
| Props | Instructions on how to build a LEGO piece | 乐高积木上的说明书 |
| Event | Someone pressing a doorbell | 有人按门铃 |
| Callback | Calling you back when your pizza is ready | 披萨做好了给你打电话 |
| CI/CD Pipeline | An automatic toy-making factory | 自动化玩具工厂 |
| Docker/Container | A lunchbox with everything inside | 一个装好所有东西的便当盒 |
| Environment Variables | Secret codes only the app knows | 只有程序知道的暗号 |

## Story Structure

Every project story follows this 5-act structure:

### Act 1: "What is this thing?" (WHAT)
- One-sentence elevator pitch a kid can understand
- The mascot emoji introduces itself
- Example: "Hi! I'm TodoBot, and I help people remember things they need to do!"

### Act 2: "Who are the characters?" (WHO)
- Introduce each major component as a character with personality
- Each character gets a name, emoji, and one-line role
- Keep to 3-7 characters max (group minor components)

### Act 3: "What happens when you use it?" (HOW - User Journey)
- Walk through the most common user action step by step
- "When you click the button, here's what happens behind the scenes..."
- Use numbered steps with emojis

### Act 4: "How do the characters work together?" (COLLABORATION)
- Show the data flow as a conversation between characters
- "The Waiter tells the Chef: 'Table 3 wants spaghetti!'"
- This maps to the architecture diagram

### Act 5: "Cool tricks and superpowers" (SPECIAL FEATURES)
- Highlight 2-3 interesting technical features
- Frame them as "superpowers" of the project
- "TodoBot has a superpower: it can remember your list even after you turn off the computer!"

## Language Rules

### DO
- Use short sentences (under 15 words when possible)
- Use active voice: "The server sends a message" not "A message is sent by the server"
- Use present tense: "clicks", "sends", "saves"
- Use everyday analogies: restaurant, school, playground, toy factory
- Include emojis generously (one per concept)
- Ask rhetorical questions: "Have you ever wondered what happens when you click 'Save'?"

### DON'T
- Use jargon without immediately explaining it
- Use passive voice
- Write walls of text (max 3 sentences per paragraph)
- Assume knowledge of programming concepts
- Use abbreviations without explanation (API → "the messenger")

### Bilingual Pattern
For each text block, provide both:
```
data-en="English version"
data-zh="中文版本"
```
Chinese should feel natural, not translated. Use colloquial Chinese that kids use.

## Metaphor Library by Component Type

### Frontend Components
- Page/View → A room in a house (每个页面就像房子里的一个房间)
- Button → A doorbell or light switch (按钮就像门铃)
- Form → A fill-in-the-blank worksheet (表单就像填空题)
- Modal → A pop-up book page (弹窗就像立体书翻开的一页)
- Navigation → Hallways connecting rooms (导航就像连接房间的走廊)

### Backend Components
- Controller/Handler → A receptionist (控制器就像前台接待)
- Service Layer → The office workers doing the real job (服务层就像真正干活的办公室员工)
- Repository/DAO → The librarian who finds books (仓库就像帮你找书的图书管理员)
- Validator → A spell-checker (验证器就像拼写检查器)

### Infrastructure
- CDN → Delivery trucks stationed everywhere (CDN就像到处都有的快递站)
- DNS → A phone book for websites (DNS就像网站的电话簿)
- SSL/TLS → A secret code that only sender and receiver know (加密就像只有发件人和收件人才知道的暗号)
- WebSocket → A walkie-talkie (always on) (WebSocket就像对讲机,随时能说话)

## Example Story Narratives

### Example: Todo App
```
Act 1: "Hi! I'm TodoBot! I'm like a super-powered sticky note that never falls off your fridge!"
Act 2: Characters - The Notepad (React UI), The Messenger (API), The Filing Cabinet (Database)
Act 3: "You write 'Buy milk' → The Notepad shows it → The Messenger runs to the Filing Cabinet → The Filing Cabinet saves it forever!"
Act 4: "The Notepad says 'New todo!' → Messenger says 'On it!' → Filing Cabinet says 'Saved!'"
Act 5: "Superpower: TodoBot can show your todos on your phone AND computer at the same time!"
```

### Example: E-commerce App
```
Act 1: "Welcome to ShopWorld! It's like a magical store where you can buy things without leaving your couch!"
Act 2: Characters - The Storefront (Next.js), The Cashier (Payment API), The Warehouse (Database), The Delivery Truck (Notification Service)
Act 3: "You find a cool toy → Add to cart → The Cashier counts your money → The Warehouse packs your order → The Delivery Truck tells you 'It's on the way!'"
```
