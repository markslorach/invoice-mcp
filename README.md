![invoice-mcp](.github/readme-assets/invoice-mcp-github-readme-banner.png)

# Invoice MCP

A Model Context Protocol server for creating professional PDF invoices using natural language.

## Getting Started

### Prerequisites

- Node v18 +

### 1. Installation

```bash
git clone https://github.com/markslorach/invoice-mcp.git
cd invoice-mcp/server
npm install
npm run build
```

### 2. Configuring Claude Desktop

Add the following json to your Claude Desktop config file -

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

This can be accessed in Claude Desktop settings in the Developer tab.

```json
{
  "mcpServers": {
    "Invoice MCP Server": {
      "command": "node",
      "args": ["/path/to/your/invoice-mcp/server/build/index.js"]
    }
  }
}
```

**Example path -**

```json
{
  "mcpServers": {
    "Invoice MCP Server": {
      "command": "node",
      "args": ["/Users/markslorach/Desktop/invoice-mcp/server/build/index.js"]
    }
  }
}
```

> Make sure to restart Claude Desktop for the MCP server to show in chat tools.

## Usage

### Prompt Examples

You can be brief -

```text
Create an invoice to Mike Smith for web development work
```

Or detailed -

```text
Create an invoice to Mike Smith for:
- Logo design (5 hours @ £75/hour)
- Website updates (3 hours @ £60/hour)
- Due in 14 days

123 Business Street, London, SW1A 1AA
mike@business.com
```

By using the instructions below, you'll be prompted to confirm or add any missing details. You can then confirm and create the invoice without all customer details if needed.

## Demo

![invoice-mcp-demo](.github/readme-assets/invoice-mcp-demo.gif)

### Logo Requirements

A logo is optional - if provided, it must be a **direct URL** to a JPG, PNG or WebP image file. The URL should end with the file extension (`.jpg`, `.png`) and link directly to the image, not a webpage. **SVG files are not supported.**

I recommend using an image with no padding around the logo for best results.

✅ Good: https://example.com/logo.png <br>
❌ Bad: https://example.com/logo-page or https://example.com/logo.svg

### Prompt Instructions

For consistent invoice generation, I recommend filling in and adding these instructions to a Claude Desktop project. If using another platform without project support - you can paste these instructions at the start of your conversation before creating an invoice.

```markdown
# Invoice MCP Project Instructions

## Pre-filled Business Information

### Logo

[Your logo URL here]

### Sender Details:

**Name:** [Your Business Name]  
**Address:** [Your Business Address]  
**Email:** [Your Business Email]

### Payment Information:

**Account Name:** [Your Account Name]  
**Account Number:** [Your Account Number]  
**Sort Code:** [Your Sort Code]

### Default Terms:

- Payment due within 30 days of invoice date
- **VAT Rate:** 0% (no VAT unless explicitly requested)
- **Currency:** GBP (supported currencies: GBP, USD, CAD, EUR)

### Default Notes:

"Thank you for your business."

## Processing Rules

### Confirmation Required

**ALWAYS** ask the user to confirm all invoice details before running the MCP tool and exporting the PDF - even for mock/test invoices. Present the confirmation in a clear, readable format.

### Invoice Detection

Watch for invoice-related keywords: "invoice", "bill", "charge", "billing" - users may say "invoice Joe Bloggs for..." instead of "create an invoice for..."

### Service Descriptions

- Enter services exactly as mentioned by the user
- Do NOT modify or rephrase service descriptions apart from correcting obvious spelling mistakes
- Preserve the user's original wording and terminology
- Never include the price in the title or description
- For hourly work, include the hourly rate in the description (e.g., "Web development @ £50.00/hour") so the quantity field represents hours worked

### Invoice Numbering

Create unique invoice numbers using format: `[Customer Initials]-[DD-MM-YYYY]`  
Examples:

- "John Smith" on 15th January 2024 = `JS-15-01-2024`
- "ABC Limited" on 3rd March 2024 = `AL-03-03-2024`

### Date Handling

- Default invoice date to today's date
- Default due date to 30 days from invoice date
- Accept natural language dates ("next Friday", "in 2 weeks")

### Missing Information Handling

- If customer details are incomplete, ask for what's missing
- For mock invoices, create realistic but fictional details
- Always prioritise user-provided information over defaults

### Error Prevention

- Validate that all monetary amounts are positive numbers
- Ensure dates are logical (due date after invoice date)
- Check that invoice numbers are unique within the conversation

### Mock Invoices

When asked for mock/test invoices:

- Create realistic but fictional customer details
- Use varied service types (consulting, design, development, etc.)
- Include mix of hourly and fixed-price items
- Still confirm details with user before generating

### File Output

- Default output path: Desktop
- Filename format: `invoice-{invoiceNumber}.pdf`
- Inform user of exact save location after generation

## User Experience Guidelines

### Conversation Flow

1. Gather invoice requirements
2. Present complete invoice details for confirmation
3. Generate PDF using MCP tool
4. Confirm successful creation and file location

### Error Handling

- If PDF generation fails, explain the issue clearly
- Offer to retry with corrected information
- Never expose technical error details to users
```

## Development

### Server

Build the MCP server -

```bash
cd server
npm run build
```

Test the MCP server using the inspector -

```bash
npm run inspector
```

### Preview App

Preview the invoice template at http://localhost:5173/ -

```bash
cd preview
npm run dev
```

The invoice template can be edited through `invoice-template.tsx` and `styles.ts` in the server folder - changes will automatically carry over to the preview.

## Tech Stack

### Server

- Model Context Protocol SDK
- TypeScript
- React PDF

### Preview

- React 19
- TypeScript
- React PDF
- Vite

## Local Model Support

Currently testing compatibility with local AI models via LM Studio. While the MCP server works, local models are less reliable than Claude and may return errors. I'm looking at ways to improve the code to help with this.

## Privacy

When using online AI services such as Claude Desktop, only include information you're comfortable sharing as data may be processed for model training.

## In Progress

- [ ] Support for generating quotes
- [ ] Invoice template refinements

## Contributing

Contributions are welcome! If you'd like to improve the Invoice MCP server -

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

For major changes, please open an issue first to discuss what you'd like to change.

## Licence

This project is licensed under the MIT Licence - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- [Figma Community Invoice Templates](https://www.figma.com/design/MN4zNKiM50IpphAOlRulJG/Invoice-Templates--Community-?node-id=0-1&p=f) - Invoice design inspiration
