# EZpay Architecture Documentation

## 🏗️ System Overview

EZpay is a single-page application (SPA) built with React that manages expense splitting between friends with Web3 wallet integration.

## 📊 Data Flow

```
User Action → Component → AppContext → localStorage
                ↓              ↓
            UI Update ← State Change
```

### Example: Creating a Split
1. User fills form in `CreateSplit.jsx`
2. Form calls `addSplit()` from AppContext
3. AppContext adds split to state and awards ezPoints
4. useEffect triggers localStorage update
5. All components using splits re-render
6. User sees updated transaction list

## 🗂️ State Management

### Global State (AppContext)
```javascript
{
  friends: Friend[],      // All friends
  splits: Split[],        // All transactions
  ezPoints: number        // Reward points
}
```

### Local State Examples
- Modal open/close states
- Form input values
- Dropdown visibility
- Search queries

## 🔄 Component Lifecycle

### Typical Component Flow
```javascript
1. Component mounts
   ↓
2. useEffect runs (fetch data, setup listeners)
   ↓
3. User interacts (clicks, types)
   ↓
4. Event handler updates state
   ↓
5. Component re-renders
   ↓
6. useEffect cleanup (on unmount)
```

## 🎯 Key Design Patterns

### 1. Context Provider Pattern
**File**: `src/context/AppContext.jsx`

Provides global state to all components without prop drilling.

```javascript
// Provider wraps entire app
<AppProvider>
  <App />
</AppProvider>

// Components consume context
const { friends, addFriend } = useAppContext();
```

### 2. Controlled Components
All form inputs are controlled by React state.

```javascript
const [value, setValue] = useState('');

<input 
  value={value} 
  onChange={(e) => setValue(e.target.value)} 
/>
```

### 3. Composition Pattern
Reusable components accept children and props.

```javascript
<Modal isOpen={isOpen} onClose={handleClose}>
  <ModalContent />
</Modal>
```

### 4. Custom Hooks (Potential)
Could extract common logic:
```javascript
// useLocalStorage.js
const useLocalStorage = (key, initialValue) => {
  // ... implementation
};
```

## 🔌 Web3 Integration

### Wallet Connection Flow
```
1. User clicks "Connect Wallet"
   ↓
2. WalletConnect checks for window.ethereum
   ↓
3. Requests account access
   ↓
4. User approves in MetaMask
   ↓
5. Account address stored in state
   ↓
6. Listens for account/network changes
```

### Event Listeners
```javascript
window.ethereum.on('accountsChanged', handleAccountChange);
window.ethereum.on('chainChanged', handleChainChange);
```

## 💾 Data Persistence

### localStorage Keys
- `friends` - JSON array of friend objects
- `splits` - JSON array of split objects
- `ezPoints` - String number

### Persistence Strategy
```javascript
// Write on state change
useEffect(() => {
  localStorage.setItem('key', JSON.stringify(data));
}, [data]);

// Read on mount
const [data] = useState(() => {
  const saved = localStorage.getItem('key');
  return saved ? JSON.parse(saved) : defaultValue;
});
```

## 🎨 Styling Architecture

### TailwindCSS Utility-First
- No custom CSS files
- All styling via className
- Responsive with breakpoint prefixes (`md:`, `lg:`)

### Color System
```javascript
// Semantic colors
Primary:   blue-600
Success:   emerald-600
Warning:   orange-600
Danger:    red-600
Neutral:   gray-100 to gray-900
```

### Component Styling Pattern
```javascript
// Base classes
const baseClasses = "px-4 py-2 rounded-lg font-medium";

// Conditional classes
const variantClasses = isPrimary 
  ? "bg-blue-600 text-white" 
  : "bg-gray-100 text-gray-900";

// Combined
<button className={`${baseClasses} ${variantClasses}`}>
```

## 🧩 Component Hierarchy

```
App
├── Navbar
│   ├── Logo
│   ├── Navigation Links
│   └── WalletConnect
│
├── Dashboard
│   ├── BalanceCard
│   └── TransactionItem (multiple)
│       └── TransactionDetailModal
│
├── Friends
│   ├── Friend List
│   │   └── Friend Item (multiple)
│   └── Split History
│       └── TransactionItem (multiple)
│
├── FriendProfile
│   ├── Profile Header
│   ├── Balance Card
│   └── Split History
│       └── TransactionItem (multiple)
│
├── CreateSplit
│   ├── Input (multiple)
│   └── Friend Selector
│       └── Friend Chips
│
└── AddFriend
    └── Input (multiple)
```

## 🔐 Security Considerations

### Current Implementation
- No backend - all data client-side
- localStorage is not encrypted
- Wallet connection uses standard Web3 patterns

### For Production
- [ ] Add backend API
- [ ] Implement authentication
- [ ] Encrypt sensitive data
- [ ] Add HTTPS
- [ ] Validate all inputs
- [ ] Rate limiting
- [ ] CSRF protection

## 🚀 Performance Optimization

### Current Optimizations
- Vite for fast builds
- React's built-in optimizations
- Lazy loading potential

### Future Optimizations
- [ ] React.memo for expensive components
- [ ] useMemo for expensive calculations
- [ ] useCallback for event handlers
- [ ] Code splitting with React.lazy
- [ ] Virtual scrolling for long lists

## 📱 Responsive Design

### Breakpoints (Tailwind)
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px

### Mobile-First Approach
```javascript
// Base styles for mobile
<div className="p-4 text-sm">
  
// Add desktop styles
<div className="p-4 text-sm md:p-6 md:text-base">
```

## 🔄 State Update Patterns

### Adding Data
```javascript
// Immutable update
setItems([...items, newItem]);

// With unique ID
setItems([...items, { ...newItem, id: Date.now().toString() }]);
```

### Updating Data
```javascript
// Map to update specific item
setItems(items.map(item => 
  item.id === targetId ? { ...item, ...updates } : item
));
```

### Deleting Data
```javascript
// Filter out item
setItems(items.filter(item => item.id !== targetId));
```

## 🎯 Event Handling

### Common Patterns
```javascript
// Form submission
const handleSubmit = (e) => {
  e.preventDefault();
  // Process form
};

// Input change
const handleChange = (e) => {
  const { name, value } = e.target;
  setFormData({ ...formData, [name]: value });
};

// Click outside
useEffect(() => {
  const handleClickOutside = (e) => {
    if (ref.current && !ref.current.contains(e.target)) {
      setIsOpen(false);
    }
  };
  document.addEventListener('mousedown', handleClickOutside);
  return () => document.removeEventListener('mousedown', handleClickOutside);
}, []);
```

## 📦 Build & Deployment

### Development
```bash
npm run dev  # Starts Vite dev server on port 5173
```

### Production Build
```bash
npm run build  # Creates optimized build in /dist
npm run preview  # Preview production build
```

### Deployment Options
- Vercel (recommended for Vite)
- Netlify
- GitHub Pages
- Any static hosting

## 🔧 Extension Points

### Easy to Add
1. **New Page**: Add file in `src/pages/`, add route
2. **New Component**: Add file in `src/components/ui/`
3. **New State**: Add to AppContext
4. **New Route**: Add to `src/App.jsx`

### Requires More Work
1. **Backend Integration**: Add API calls, auth
2. **Real Payments**: Smart contract integration
3. **Real-time Updates**: WebSocket or polling
4. **File Uploads**: Add storage service

## 🐛 Debugging Tips

### Common Issues
1. **State not updating**: Check if using immutable updates
2. **Component not re-rendering**: Verify dependencies in useEffect
3. **localStorage not persisting**: Check useEffect dependencies
4. **Wallet not connecting**: Verify MetaMask is installed

### Debug Tools
- React DevTools (Components, Profiler)
- Browser DevTools (Console, Network, Application)
- localStorage inspector
- MetaMask console logs

## 📈 Scalability Considerations

### Current Limitations
- All data in localStorage (5-10MB limit)
- No pagination for large lists
- No backend for data sync

### Scaling Solutions
1. Add backend API
2. Implement pagination
3. Add database
4. Cache frequently accessed data
5. Optimize re-renders with React.memo

## 🎓 Learning Path

### For New Contributors
1. Understand React hooks (useState, useEffect, useContext)
2. Learn TailwindCSS basics
3. Study AppContext pattern
4. Review component structure
5. Understand Web3 wallet connection
6. Practice with small features first

### Recommended Order
1. Add simple UI component
2. Add new page with routing
3. Add state to AppContext
4. Integrate with existing features
5. Add Web3 functionality

---

This architecture is designed to be:
- **Simple**: Easy to understand and modify
- **Scalable**: Can grow with new features
- **Maintainable**: Clear patterns and structure
- **AI-Friendly**: Well-documented for AI assistance
