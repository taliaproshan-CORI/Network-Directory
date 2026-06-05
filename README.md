# Network-Directory

CORI Network Directory
A searchable internal directory of Matt's LinkedIn contacts, built to help the CORI team quickly match founder needs with the right people in the network.
What it does
The site loads a live database of contacts from a connected Google Sheet and displays them as searchable cards. Team members can search by name, company, job title, or area of expertise. The search is semantic — meaning related terms are matched automatically, so searching "consulting" surfaces McKinsey analysts, searching "hiring" surfaces talent agency contacts, and so on.
How to update contacts
Open the connected Google Sheet and add, edit, or remove rows. Changes appear on the site automatically the next time someone loads the page. No code changes needed.
The sheet must have these column headers in row 1:
ColumnDescriptionFirst NameContact's first nameLast NameContact's last nameURLLinkedIn profile URLCompanyCurrent employerPositionCurrent job titleNotesAny additional context
How the code works
1. Loading data (JSONP)
When the page loads, it fires a JSONP request to a Google Apps Script deployment. JSONP works by injecting a <script> tag that calls the Apps Script URL — Google wraps the response in a function call (handleData([...])) which the browser executes automatically. This bypasses the CORS restrictions that block normal fetch requests to Google's servers.
2. Parsing contacts
Each row from the sheet is mapped into a clean contact object. All values are wrapped in String() to handle cases where Google Sheets returns numbers or dates instead of text. Rows with no name are filtered out.
3. Auto-categorization
Each contact is automatically tagged with one or more of four categories based on keywords in their job title:

Fundraising & Capital Access — investor, venture, capital, fund, banking, equity, etc.
Hiring & Talent — talent, recruit, hiring, HR, people ops, staffing, etc.
Product-Market Fit — product, growth, marketing, GTM, sales, founder, CEO, etc.
Legal & Compliance — counsel, legal, attorney, compliance, regulatory, etc.

No manual tagging needed — it runs automatically from the Position field.
4. Semantic search
A built-in dictionary (SEMANTIC_MAP) maps search terms to related terms. When someone types a query, the code expands it into all related terms before searching. Examples:

"consulting" → also matches McKinsey, Bain, BCG, Deloitte, advisor, strategy, analyst
"Goldman" → also matches investment banking, finance, analyst, trading, securities
"hiring" → also matches recruiter, talent, HR, people ops, staffing, executive search
"vc" → also matches venture capital, fund, investor, angel, seed, partner

5. Rendering
Cards are built dynamically from the filtered contact list. Clicking a card opens a modal with full contact details and a LinkedIn link. The category filter dropdown and filter tag buttons narrow results in real time.
6. Avatar colors
Each contact gets a consistent avatar color based on a hash of their name — so the same person always gets the same color across page loads without storing anything.
Tech stack

Plain HTML, CSS, JavaScript — no frameworks, no build step
Hosted on GitHub Pages
Data sourced from Google Sheets via Google Apps Script (JSONP)
Fonts: Inter (Google Fonts)
Icons: Tabler Icons

Hosting
The site is a single index.html file hosted on GitHub Pages. Any changes committed to the main branch go live within about 60 seconds.
To connect a new Google Sheet

Open the new sheet
Click Extensions → Apps Script
Paste the Apps Script code (see below)
Deploy as a Web App (Execute as: Me, Who has access: Anyone)
Copy the /exec URL
Replace the SHEET_API_URL value in index.html

Apps Script code
javascriptfunction doGet(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getDataRange().getValues();
  const headers = data[0];
  const rows = data.slice(1).map(row => {
    const obj = {};
    headers.forEach((h, i) => obj[h] = row[i]);
    return obj;
  }).filter(row => row[headers[0]]);
  const json = JSON.stringify(rows);
  const callback = e && e.parameter && e.parameter.callback;
  if (callback) {
    return ContentService
      .createTextOutput(callback + '(' + json + ')')
      .setMimeType(ContentService.MimeType.JAVASCRIPT);
  }
  return ContentService
    .createTextOutput(json)
    .setMimeType(ContentService.MimeType.JSON);
}
