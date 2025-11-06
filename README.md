# EZpay - Split Payment Application

A modern, Web3-enabled expense splitting application built with React, allowing users to manage shared expenses with friends and connect their crypto wallets.

## 🚀 Features

- **Split Expenses**: Create and manage split payments with multiple friends
- **Web3 Wallet Integration**: Connect MetaMask and other Web3 wallets
- **Friend Management**: Add friends with contact details and Ethereum addresses
- **Transaction History**: View detailed transaction history with participant information
- **ezPoints Rewards**: Earn 20 ezPoints for every transaction created
- **Balance Tracking**: Track amounts owed and due from friends
- **Responsive Design**: Modern UI with TailwindCSS

## 📁 Project Structure

```
splitwise-app/
├── src/
│   ├── components/
│   │   ├── layout/
│   │   │   └── Navbar.jsx           # Main navigation with wallet connect
│   │   └── ui/
│   │       ├── BalanceCard.jsx      # Account balance display card
│   │       ├── Input.jsx            # Reusable input component
│   │       ├── TransactionItem.jsx  # Transaction list item
│   │       ├── TransactionDetailModal.jsx  # Transaction details popup
│   │       └── WalletConnect.jsx    # Web3 wallet connection
│   ├── context/
│   │   └── AppContext.jsx           # Global state management
│   ├── pages/
│   │   ├── Dashboard.jsx            # Main account page
│   │   ├── Friends.jsx              # Friends list and split history
│   │   ├── FriendProfile.jsx        # Individual friend details
│   │   ├── AddFriend.jsx            # Add new friend form
│   │   └── CreateSplit.jsx          # Create new split transaction
│   ├── App.jsx                      # Main app component with routes
│   └── main.jsx                     # App entry point
├── public/                          # Static assets
└── package.json                     # Dependencies and scripts
```

## 🛠️ Tech Stack

- **React 18** - UI framework
- **React Router** - Client-side routing
- **TailwindCSS** - Styling
- **Ethers.js** - Web3 wallet integration
- **Lucide React** - Icon library
- **Vite** - Build tool and dev server

## 📦 Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## 🔑 Key Components

### Context (AppContext.jsx)
- Manages global state for friends, splits, and ezPoints
- Persists data to localStorage
- Provides functions: `addFriend`, `addSplit`, `deleteSplit`, `getTotalBalance`, `getEzPoints`

### Pages

#### Dashboard.jsx
- Displays account balance card
- Shows transaction history
- Action buttons: Send Payment, Share Invite

#### Friends.jsx
- Split-screen layout with friends list and split history
- Shows friend balances (Due/Settled status)
- Click friends to view profiles

#### FriendProfile.jsx
- Detailed friend information
- Balance summary
- Friend-specific transaction history

#### CreateSplit.jsx
- Form to create new split transactions
- Friend selection with search and chips
- Automatically calculates per-person amounts
- Awards 20 ezPoints on creation

#### AddFriend.jsx
- Form to add new friends
- Fields: Name, Nickname, Email, Ethereum Address, Telegram

### Components

#### WalletConnect.jsx
- Connects to MetaMask and other Web3 wallets
- Displays connected address
- Handles account changes and disconnection

#### BalanceCard.jsx
- Shows user's total balance
- Action buttons for payments and sharing

#### TransactionDetailModal.jsx
- Popup showing full transaction details
- Participant list with payment status (Paid/Due)
- Delete and Settle Up actions

## 💾 Data Structure

### Friend Object
```javascript
{
  id: string,
  name: string,
  nickname: string,
  email: string,
  ethereumAddress: string,
  telegram: string
}
```

### Split Object
```javascript
{
  id: string,
  title: string,
  amount: string,           // Total amount
  perPersonAmount: string,  // Amount per person
  paidBy: 'me',
  paidByName: 'You',
  participants: [           // Array of friend objects
    { id: string, name: string }
  ],
  date: string              // ISO date string
}
```

## 🎨 Styling

- Uses TailwindCSS utility classes
- Gradient backgrounds for avatars and cards
- Responsive design with mobile-first approach
- Consistent color scheme:
  - Blue: Primary actions
  - Green/Emerald: Success, settled balances
  - Red/Orange: Due amounts, warnings
  - Gray: Neutral elements

## 🔐 Web3 Integration

- Detects installed wallet extensions
- Requests account access
- Listens for account and network changes
- Displays shortened addresses (0xabcd...1234)

## 📊 State Management

All state is managed through React Context (AppContext):
- **friends**: Array of friend objects
- **splits**: Array of split transaction objects
- **ezPoints**: Integer count of reward points

Data persists in localStorage with keys:
- `friends`
- `splits`
- `ezPoints`

## 🎯 ezPoints System

- Users earn 20 ezPoints per transaction
- Points display in navbar
- Stored as integer (no decimals)
- Persists across sessions

## 🚧 Future Enhancement Ideas

- Payment settlement with crypto
- Transaction notifications
- Export transaction history
- Multi-currency support
- Group expenses
- Receipt uploads
- Analytics dashboard

## 📝 Development Notes

- All components use functional components with hooks
- State updates trigger localStorage persistence
- Avatar colors generated from name hash
- Transaction amounts calculated automatically
- Click-outside handlers for dropdowns

## 🤝 Contributing

When adding features:
1. Follow existing component structure
2. Use TailwindCSS for styling
3. Add to AppContext if global state needed
4. Update this README with new features
5. Test wallet connection functionality

## 📄 License

MIT
