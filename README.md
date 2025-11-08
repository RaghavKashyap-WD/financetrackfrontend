# Personal Finance Tracker

A production-ready personal finance management application built with Next.js, Tailwind CSS, and shadcn/ui. Track earnings, expenses, set budgets, and gain insights into your spending patterns with AI-powered forecasting.

## Features

- **Authentication**: Secure email/password login with JWT tokens stored in HttpOnly cookies
- **Dashboard**: Summary cards, quick-add forms, monthly charts, and AI forecast widget
- **Transaction Management**: Full CRUD operations with search, filtering, and sorting
- **Settings**: Profile management, budget configuration, and custom expense categories
- **Charts & Analytics**: Visual representations of earnings vs expenses and category breakdowns
- **Responsive Design**: Mobile-first approach with sidebar navigation (desktop) and bottom nav (mobile)
- **Dark/Light Theme**: Toggle between themes with persistent storage
- **Data Export**: Export transactions to CSV format

## Tech Stack

- **Frontend**: Next.js 16 (App Router) with React 19
- **Styling**: Tailwind CSS v4
- **UI Components**: shadcn/ui
- **Charts**: Recharts
- **Data Fetching**: SWR
- **Type Safety**: TypeScript
- **Icons**: Lucide React

## Project Structure

\`\`\`
├── app/
│   ├── auth/               # Authentication pages (login, signup)
│   ├── dashboard/          # Main dashboard page
│   ├── transactions/       # Transaction management page
│   ├── settings/           # Settings and preferences
│   ├── api/                # API routes
│   ├── layout.tsx          # Root layout with metadata
│   └── globals.css         # Global styles and theme tokens
├── components/
│   ├── layout/             # Layout components (sidebar, header)
│   ├── charts/             # Chart components (Recharts)
│   ├── ui/                 # shadcn/ui primitives (auto-generated)
│   ├── transaction-form.tsx    # Reusable form for adding transactions
│   ├── transaction-table.tsx   # Table displaying transactions
│   └── summary-card.tsx        # Summary statistics card
├── lib/
│   ├── auth-utils.ts       # Authentication utilities
│   ├── chart-utils.ts      # Chart data transformation
│   ├── format-utils.ts     # Formatting and utility functions
│   ├── validation-utils.ts # Form and data validation
│   ├── api-utils.ts        # API call helpers
│   ├── storage-utils.ts    # LocalStorage utilities
│   └── utils.ts            # shadcn utility (cn function)
├── hooks/
│   ├── use-transactions.ts # SWR hook for transactions
│   ├── use-summary.ts      # SWR hook for summary data
│   ├── use-mobile.tsx      # Mobile responsiveness hook
│   └── use-toast.ts        # Toast notifications
├── middleware.ts           # Route protection middleware
└── tsconfig.json
\`\`\`

## Getting Started

### Installation

1. **Clone and Install**
   \`\`\`bash
   # Download or extract the project
   cd personal-finance-tracker

   # The Next.js runtime automatically installs dependencies from imports
   # No need to run npm install manually
   \`\`\`

2. **Environment Variables**
   Create a `.env.local` file in the root directory:
   \`\`\`env
   # API endpoint (if using external backend)
   NEXT_PUBLIC_API_URL=http://localhost:3000

   # JWT secret for authentication
   JWT_SECRET=your-secret-key-here

   # Optional: Database connection
   DATABASE_URL=your-database-url
   \`\`\`

3. **Run Development Server**
   \`\`\`bash
   npm run dev
   \`\`\`
   Open [http://localhost:3000](http://localhost:3000) in your browser.

### Default Credentials (for testing)

The authentication system is set up with mock endpoints. In production, connect to your backend:
- API routes are in `app/api/`
- Mock data structure: `{ email, password, name }`

## API Endpoints

The app expects these backend endpoints:

### Authentication
- `POST /api/auth/signup` - Create new account
- `POST /api/auth/login` - Login with email/password
- `POST /api/auth/logout` - Logout and clear session
- `GET /api/user/me` - Get current user info

### Transactions
- `GET /api/transactions?month=YYYY-MM&page=1` - List transactions
- `POST /api/transactions` - Create transaction
- `PUT /api/transactions/:id` - Update transaction
- `DELETE /api/transactions/:id` - Delete transaction

### Summary & Analytics
- `GET /api/summary/monthly?month=YYYY-MM` - Monthly summary
- `POST /api/ai/plan` - AI financial forecast

### User Settings
- `PUT /api/user/profile` - Update profile info
- `PUT /api/user/budget` - Set budget limits
- `PUT /api/user/categories` - Update expense categories

## Key Components

### Layout Components
- **Sidebar**: Navigation with menu items, collapsible on mobile
- **Header**: Theme toggle and mobile navigation trigger

### Form Components
- **TransactionForm**: Add earnings or expenses with validation
- **SummaryCard**: Display financial metrics with trends
- **TransactionTable**: View, edit, and delete transactions

### Chart Components
- **ExpenseEarningsChart**: Line/bar chart comparing income vs expenses
- **CategoryBreakdownChart**: Pie chart showing expense distribution

## Utilities

### Format Utils
- `formatCurrency(amount, currency)` - Format numbers as currency
- `formatDate(date)` - Format date strings
- `formatMonth(dateString)` - Format year-month
- `calculatePercentageChange(current, previous)` - Calculate trends

### Validation Utils
- `isValidEmail(email)` - Email validation
- `isValidPassword(password)` - Password requirements (8+ chars)
- `isValidAmount(amount)` - Positive number validation
- `getPasswordStrength(password)` - Password strength indicator

### API Utils
- `apiCall<T>(url, options)` - Type-safe API wrapper
- `apiGet/Post/Put/Delete<T>()` - Helper methods
- `buildQueryString(params)` - URL query builder

### Custom Hooks
- `useTransactions(month, page)` - Fetch transactions with SWR
- `useSummary(month)` - Fetch monthly summary

## Authentication Flow

1. User signs up with email, password, and name
2. Backend creates user and returns JWT token
3. Token stored in HttpOnly cookie (secure by default)
4. Middleware checks for valid token on protected routes
5. On logout, cookie is deleted and user redirected to login

## Styling & Theme

The app uses Tailwind CSS v4 with semantic design tokens defined in `globals.css`:

\`\`\`css
:root {
  --primary: oklch(0.205 0 0);           /* Dark blue */
  --secondary: oklch(0.97 0 0);          /* Light gray */
  --destructive: oklch(0.577 0.245 27);  /* Red for errors */
  /* ... more tokens ... */
}

.dark {
  --primary: oklch(0.985 0 0);           /* Light blue for dark mode */
  /* ... dark mode overrides ... */
}
\`\`\`

Access theme via Tailwind classes: `bg-primary`, `text-primary-foreground`, etc.

## Development Tips

### Adding New Pages
1. Create file in `app/[page]/page.tsx`
2. Wrap in `<MainLayout>` component
3. Add navigation link in `components/layout/sidebar.tsx`

### Adding API Routes
1. Create file in `app/api/[route]/route.ts`
2. Export `GET`, `POST`, `PUT`, `DELETE` functions
3. Handle authentication/authorization in route

### Using Charts
\`\`\`tsx
import { ExpenseEarningsChart } from '@/components/charts';

const data = transformMonthlyData(transactions);
<ExpenseEarningsChart data={data} type="line" />
\`\`\`

### Fetching Data
\`\`\`tsx
import useSWR from 'swr';

const { data, isLoading, mutate } = useSWR('/api/transactions', fetcher);
\`\`\`

## Performance Optimizations

- **Code Splitting**: Dynamic imports for heavy components
- **Image Optimization**: Next.js Image component for all images
- **Caching**: SWR for client-side caching
- **Middleware**: Route protection without API overhead
- **CSS-in-JS**: Tailwind for optimal CSS delivery

## Deployment

### Deploy to Vercel (Recommended)

1. Push code to GitHub
2. Connect repository to Vercel
3. Add environment variables in Vercel dashboard
4. Deploy automatically

\`\`\`bash
npm run build
npm run start
\`\`\`

### Docker

\`\`\`dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
EXPOSE 3000
CMD ["npm", "run", "start"]
\`\`\`

## Security Best Practices

- JWT tokens stored in HttpOnly cookies (not localStorage)
- Middleware protection for sensitive routes
- Password validation (minimum 8 characters)
- Email validation
- CSRF protection via Next.js middleware
- Secure API endpoints with authentication checks

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile browsers (iOS Safari, Chrome Mobile)

## Troubleshooting

### Authentication Issues
- Clear cookies and localStorage
- Check JWT secret matches between frontend/backend
- Verify token expiration settings

### Chart Not Displaying
- Ensure transaction data is in correct format
- Check browser console for errors
- Verify Recharts is properly imported

### API Errors
- Check NEXT_PUBLIC_API_URL matches backend
- Verify CORS headers in backend
- Check network tab in DevTools

## Future Enhancements

- [ ] Multi-currency support
- [ ] Recurring transactions
- [ ] Receipt upload and OCR
- [ ] Automated categorization with AI
- [ ] Budget alerts and notifications
- [ ] Bank account integration
- [ ] Investment tracking
- [ ] Collaborative budgeting

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - feel free to use this project for personal or commercial purposes.

## Support

For issues or questions:
- Check the troubleshooting section
- Review API endpoint documentation
- Check browser console for errors
- Open an issue on GitHub

## Changelog

### v1.0.0
- Initial release
- Authentication system
- Dashboard with summary cards
- Transaction management
- Settings and preferences
- Chart visualizations
- Responsive design
#   f i n a n c e t r a c k f r o n t e n d  
 