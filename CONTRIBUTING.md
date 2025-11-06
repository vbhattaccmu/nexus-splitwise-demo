# Contributing to EZpay

Thank you for your interest in contributing to EZpay! This guide will help you understand the codebase and make meaningful contributions.

## 🎯 Quick Start for AI-Assisted Development

This project is optimized for AI-assisted development. Here are key things to know:

### Component Architecture
- **Functional Components**: All components use React hooks (useState, useEffect, useContext)
- **Context Pattern**: Global state managed via AppContext
- **Component Composition**: Reusable UI components in `src/components/ui/`

### Code Patterns to Follow

#### 1. State Management
```javascript
// Always use AppContext for global state
const { friends, splits, addFriend, addSplit } = useAppContext();

// Use local state for component-specific data
const [isOpen, setIsOpen] = useState(false);
```

#### 2. Styling
```javascript
// Use TailwindCSS utility classes
<div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6">

// For conditional styling
<div className={`text-sm ${isActive ? 'text-blue-600' : 'text-gray-600'}`}>
```

#### 3. Event Handlers
```javascript
// Prefix with 'handle'
const handleClick = () => { ... };
const handleSubmit = (e) => { e.preventDefault(); ... };
```

## 📂 File Organization

### Where to Add New Features

| Feature Type | Location | Example |
|-------------|----------|---------|
| New Page | `src/pages/` | `Analytics.jsx` |
| Reusable UI Component | `src/components/ui/` | `Button.jsx` |
| Layout Component | `src/components/layout/` | `Footer.jsx` |
| Global State | `src/context/AppContext.jsx` | Add new state/functions |
| Route | `src/App.jsx` | Add `<Route>` element |

## 🔧 Common Tasks

### Adding a New Page

1. Create file in `src/pages/`:
```javascript
import React from 'react';

const NewPage = () => {
  return (
    <div className="max-w-4xl mx-auto space-y-8">
      <h1 className="text-2xl font-bold">New Page</h1>
    </div>
  );
};

export default NewPage;
```

2. Add route in `src/App.jsx`:
```javascript
import NewPage from './pages/NewPage';

// In Routes
<Route path="/new-page" element={<NewPage />} />
```

3. Add navigation link in `src/components/layout/Navbar.jsx`:
```javascript
const navItems = [
  { name: 'Account', path: '/', icon: CreditCard },
  { name: 'Friends', path: '/friends', icon: Users },
  { name: 'New Page', path: '/new-page', icon: SomeIcon },
];
```

### Adding Global State

Edit `src/context/AppContext.jsx`:

```javascript
// 1. Add state
const [newData, setNewData] = useState(() => {
  const saved = localStorage.getItem('newData');
  return saved ? JSON.parse(saved) : [];
});

// 2. Add persistence
useEffect(() => {
  localStorage.setItem('newData', JSON.stringify(newData));
}, [newData]);

// 3. Add functions
const addNewData = (data) => {
  setNewData([...newData, { ...data, id: Date.now().toString() }]);
};

// 4. Export in Provider
<AppContext.Provider value={{ 
  friends, splits, newData,
  addFriend, addSplit, addNewData 
}}>
```

### Creating a Reusable Component

```javascript
// src/components/ui/Card.jsx
import React from 'react';

const Card = ({ title, children, className = '' }) => {
  return (
    <div className={`bg-white rounded-xl shadow-sm border border-gray-100 p-6 ${className}`}>
      {title && <h3 className="text-lg font-semibold mb-4">{title}</h3>}
      {children}
    </div>
  );
};

export default Card;
```

## 🎨 Design System

### Colors
- **Primary**: Blue (`blue-600`, `blue-700`)
- **Success**: Green/Emerald (`emerald-600`, `green-500`)
- **Warning**: Orange (`orange-600`)
- **Danger**: Red (`red-600`)
- **Neutral**: Gray (`gray-100` to `gray-900`)

### Spacing
- Use Tailwind spacing scale: `p-4`, `m-6`, `space-y-8`
- Container max-width: `max-w-4xl mx-auto`

### Typography
- Headings: `text-2xl font-bold` to `text-5xl font-bold`
- Body: `text-sm` to `text-base`
- Labels: `text-xs text-gray-500`

### Components
- Rounded corners: `rounded-lg` or `rounded-xl`
- Shadows: `shadow-sm` or `shadow-lg`
- Borders: `border border-gray-100`

## 🔍 Understanding Key Files

### AppContext.jsx
**Purpose**: Central state management
**Key Functions**:
- `addFriend(friend)` - Adds new friend
- `addSplit(split)` - Creates transaction, awards ezPoints
- `deleteSplit(id)` - Removes transaction
- `getTotalBalance()` - Calculates total from splits
- `getEzPoints()` - Returns current points

### WalletConnect.jsx
**Purpose**: Web3 wallet integration
**Key Features**:
- Detects MetaMask/Web3 wallets
- Connects/disconnects wallet
- Listens for account changes
- Displays shortened address

### TransactionDetailModal.jsx
**Purpose**: Shows transaction details
**Props**:
- `transaction` - Split object
- `isOpen` - Boolean
- `onClose` - Function
- `onDelete` - Function

## 🧪 Testing Checklist

Before submitting changes:
- [ ] Component renders without errors
- [ ] Responsive on mobile and desktop
- [ ] State updates correctly
- [ ] localStorage persists data
- [ ] No console errors
- [ ] Follows existing code style
- [ ] Works with wallet connected/disconnected

## 💡 AI Prompting Tips

When using AI to add features:

### Good Prompts
✅ "Add a search bar to the Friends page that filters friends by name"
✅ "Create a modal to edit friend details with form validation"
✅ "Add a chart showing spending over time using the splits data"

### Include Context
✅ "Using the existing TransactionItem component, add a feature to..."
✅ "Following the pattern in CreateSplit.jsx, create a form for..."
✅ "Update AppContext to include a new function that..."

### Specify Requirements
✅ "Use TailwindCSS for styling"
✅ "Store data in localStorage"
✅ "Add to the existing AppContext"
✅ "Follow the component structure in src/components/ui/"

## 🚀 Feature Ideas

### Easy
- Add dark mode toggle
- Export transactions to CSV
- Add transaction categories
- Friend search functionality

### Medium
- Multi-currency support
- Transaction filters and sorting
- Spending analytics dashboard
- Email/Telegram notifications

### Advanced
- Actual crypto payments via smart contracts
- Group expenses (more than 2 people)
- Receipt image uploads
- Recurring payments

## 📚 Resources

- [React Docs](https://react.dev/)
- [TailwindCSS Docs](https://tailwindcss.com/docs)
- [Ethers.js Docs](https://docs.ethers.org/)
- [React Router Docs](https://reactrouter.com/)

## 🐛 Reporting Issues

When reporting bugs, include:
1. Steps to reproduce
2. Expected behavior
3. Actual behavior
4. Browser and wallet used
5. Console errors (if any)

## 📝 Code Style

- Use functional components with hooks
- Destructure props and context
- Use meaningful variable names
- Add comments for complex logic
- Keep components under 300 lines
- Extract reusable logic into custom hooks

## 🤝 Pull Request Process

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Update README if needed
6. Submit PR with clear description

---

Happy coding! If you have questions, feel free to open an issue.
