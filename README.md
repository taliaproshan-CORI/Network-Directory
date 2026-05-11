# Network-Directory

Portfolio Network — Expert Directory
An internal tool for surfacing the right LP, Board Member, and other contacts when portfolio founders need help.
What it does
The site hosts a searchable database of LP contacts and domain experts. Team members can search by name, company, job title, or area of expertise — the directory instantly returns anyone in the database who matches. Each result shows the person's role, the founder pain point categories they can address, and their contact information.
Who it's for
The internal team uses this on behalf of founders. When a founder flags a need — whether that's fundraising advice, a key hire, legal guidance, or help finding product-market fit — the team searches the directory to identify the right LP to loop in.
Features

Keyword search — search across names, companies, titles, and bios
Category filter — filter by the four founder pain point categories: Fundraising & Capital Access, Hiring & Talent, Product-Market Fit, and Legal & Compliance
Quick-filter tags — one-click filters for common expertise areas
Contact cards — click any result to see full contact details and copy them with one click
How It Works page — documents the full LP communication workflow for the team

How to update the database
All contact data lives in the experts array inside the <script> block near the bottom of index.html. Each person follows this structure:
js{
  id: 1,
  name: "Full Name",
  title: "Title, Company",
  company: "Company Name",
  email: "email@example.com",
  phone: "(000) 000-0000",
  categories: ["Fundraising & Capital Access", "Hiring & Talent"],
  expertise: ["Tag One", "Tag Two", "Tag Three"],
  bio: "A short description of this person's background and what they can help with.",
  avatar: "AB",
  avatarClass: "avatar-green"
}
Available avatarClass options: avatar-green, avatar-blue, avatar-amber, avatar-coral, avatar-purple
Hosting
The site is hosted via GitHub Pages. Any changes committed to the main branch go live automatically within about a minute.
