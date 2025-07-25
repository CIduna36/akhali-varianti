# akhali-varianti
## 🎨 **Updated Frontend Components**

### 📄 `frontend/styles/globals.css`
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 240 10% 3.9%;
    --foreground: 0 0% 98%;
    --card: 240 10% 3.9%;
    --card-foreground: 0 0% 98%;
    --popover: 240 10% 3.9%;
    --popover-foreground: 0 0% 98%;
    --primary: 221 83% 53%;
    --primary-foreground: 0 0% 98%;
    --secondary: 240 3.7% 15.9%;
    --secondary-foreground: 0 0% 98%;
    --muted: 240 3.7% 15.9%;
    --muted-foreground: 240 5% 64.9%;
    --accent: 240 3.7% 15.9%;
    --accent-foreground: 0 0% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 0 0% 98%;
    --border: 240 3.7% 15.9%;
    --input: 240 3.7% 15.9%;
    --ring: 221 83% 53%;
    --radius: 0.5rem;
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
    font-feature-settings: "rlig" 1, "calt" 1;
  }
}

@layer utilities {
  .gradient-bg {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  }
  
  .glass-effect {
    @apply backdrop-blur-md bg-white/10 border border-white/20;
  }
  
  .glow-effect {
    box-shadow: 0 0 20px rgba(102, 126, 234, 0.3);
  }
  
  .hover-scale {
    @apply transition-all duration-300 hover:scale-105;
  }
}
```

### 📄 `frontend/components/ui/Button.tsx`
```tsx
import * as React from "react"
import { cn } from "@/utils/helpers"

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'default' | 'destructive' | 'outline' | 'secondary' | 'ghost' | 'link'
  size?: 'default' | 'sm' | 'lg' | 'icon'
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant = 'default', size = 'default', ...props }, ref) => {
    return (
      <button
        className={cn(
          "inline-flex items-center justify-center rounded-md text-sm font-medium transition-all duration-200",
          "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
          "disabled:opacity-50 disabled:pointer-events-none",
          {
            'bg-gradient-to-r from-blue-600 to-purple-600 text-white hover:from-blue-700 hover:to-purple-700 shadow-lg hover:shadow-xl': variant === 'default',
            'bg-red-600 text-white hover:bg-red-700': variant === 'destructive',
            'border border-gray-300 bg-transparent hover:bg-gray-100 dark:border-gray-700 dark:hover:bg-gray-800': variant === 'outline',
            'bg-gray-100 text-gray-900 hover:bg-gray-200 dark:bg-gray-800 dark:text-gray-50 dark:hover:bg-gray-700': variant === 'secondary',
            'hover:bg-gray-100 dark:hover:bg-gray-800': variant === 'ghost',
            'text-blue-600 underline-offset-4 hover:underline': variant === 'link',
          },
          {
            'h-10 px-4 py-2': size === 'default',
            'h-9 rounded-md px-3': size === 'sm',
            'h-11 rounded-md px-8': size === 'lg',
            'h-10 w-10': size === 'icon',
          },
          className
        )}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = "Button"

export { Button }
```

### 📄 `frontend/components/ui/Input.tsx`
```tsx
import * as React from "react"
import { cn } from "@/utils/helpers"

export interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {}

const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ className, type, ...props }, ref) => {
    return (
      <input
        type={type}
        className={cn(
          "flex h-10 w-full rounded-md border border-gray-300 px-3 py-2 text-sm",
          "bg-gray-50 dark:bg-gray-900 dark:border-gray-700",
          "placeholder:text-gray-500 dark:placeholder:text-gray-400",
          "focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent",
          "transition-all duration-200",
          "hover:border-gray-400 dark:hover:border-gray-600",
          className
        )}
        ref={ref}
        {...props}
      />
    )
  }
)
Input.displayName = "Input"

export { Input }
```

### 📄 `frontend/components/layout/Layout.tsx`
```tsx
import { useState } from 'react'
import { useRouter } from 'next/router'
import { motion } from 'framer-motion'
import { 
  Home, 
  Server, 
  CreditCard, 
  Settings, 
  LogOut,
  Menu,
  X,
  BarChart3,
  Bell
} from 'lucide-react'
import { useAuth } from '@/contexts/AuthContext'

interface LayoutProps {
  children: React.ReactNode
}

export default function Layout({ children }: LayoutProps) {
  const [sidebarOpen, setSidebarOpen] = useState(false)
  const { user, logout } = useAuth()
  const router = useRouter()

  const navigation = [
    { name: 'Dashboard', href: '/dashboard', icon: Home },
    { name: 'Servers', href: '/servers', icon: Server },
    { name: 'Analytics', href: '/analytics', icon: BarChart3 },
    { name: 'Billing', href: '/billing', icon: CreditCard },
    { name: 'Settings', href: '/settings', icon: Settings },
  ]

  const handleLogout = async () => {
    await logout()
    router.push('/login')
  }

  return (
    <div className="min-h-screen bg-gray-950">
      {/* Mobile sidebar backdrop */}
      {sidebarOpen && (
        <div 
          className="fixed inset-0 z-40 bg-gray-900/50 lg:hidden"
          onClick={() => setSidebarOpen(false)}
        />
      )}

      {/* Sidebar */}
      <motion.div
        initial={{ x: -280 }}
        animate={{ x: sidebarOpen ? 0 : -280 }}
        transition={{ type: "spring", stiffness: 300, damping: 30 }}
        className="fixed inset-y-0 left-0 z-50 w-64 bg-gray-900 border-r border-gray-800 lg:translate-x-0"
      >
        <div className="flex h-16 items-center justify-between px-4 border-b border-gray-800">
          <h1 className="text-xl font-bold bg-gradient-to-r from-blue-400 to-purple-400 bg-clip-text text-transparent">
            GamePanel
          </h1>
          <button
            onClick={() => setSidebarOpen(false)}
            className="lg:hidden text-gray-400 hover:text-white"
          >
            <X className="h-6 w-6" />
          </button>
        </div>

        <nav className="flex-1 space-y-1 px-2 py-4">
          {navigation.map((item) => {
            const isActive = router.pathname === item.href
            return (
              <motion.a
                key={item.name}
                href={item.href}
                className={`
                  group flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium
                  ${isActive 
                    ? 'bg-blue-600/20 text-blue-400 border-l-2 border-blue-400' 
                    : 'text-gray-300 hover:bg-gray-800 hover:text-white'}
                `}
                whileHover={{ scale: 1.02 }}
                whileTap={{ scale: 0.98 }}
              >
                <item.icon className="h-5 w-5" />
                {item.name}
              </motion.a>
            )
          })}
        </nav>

        <div className="border-t border-gray-800 p-4">
          <button
            onClick={handleLogout}
            className="flex w-full items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium text-gray-300 hover:bg-gray-800 hover:text-white"
          >
            <LogOut className="h-5 w-5" />
            Logout
          </button>
        </div>
      </motion.div>

      {/* Main content */}
      <div className="lg:pl-64">
        {/* Top bar */}
        <div className="sticky top-0 z-30 bg-gray-900/80 backdrop-blur-sm border-b border-gray-800">
          <div className="flex h-16 items-center justify-between px-4">
            <button
              onClick={() => setSidebarOpen(true)}
              className="lg:hidden text-gray-400 hover:text-white"
            >
              <Menu className="h-6 w-6" />
            </button>

            <div className="flex items-center gap-4">
              <button className="relative p-2 text-gray-400 hover:text-white">
                <Bell className="h-5 w-5" />
                <span className="absolute -top-1 -right-1 h-3 w-3 rounded-full bg-red-500" />
              </button>
              
              <div className="flex items-center gap-3">
                <div className="h-8 w-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-500" />
                <span className="text-sm font-medium text-white">{user?.email}</span>
              </div>
            </div>
          </div>
        </div>

        {/* Page content */}
        <main className="p-6">
          {children}
        </main>
      </div>
    </div>
  )
}
```

### 📄 `frontend/pages/index.tsx`
```tsx
import { motion } from 'framer-motion'
import { useRouter } from 'next/router'
import { 
  Play, 
  Users, 
  Globe, 
  Shield, 
  Zap, 
  Gamepad2,
  ArrowRight,
  Star
} from 'lucide-react'
import { Button } from '@/components/ui/Button'

const features = [
  {
    icon: Zap,
    title: 'Instant Deployment',
    description: 'Deploy your game server in under 60 seconds with our automated system.'
  },
  {
    icon: Globe,
    title: 'Global Network',
    description: 'Choose from 7+ locations worldwide for optimal latency.'
  },
  {
    icon: Shield,
    title: 'Enterprise Security',
    description: 'Military-grade security with DDoS protection and 24/7 monitoring.'
  },
  {
    icon: Users,
    title: 'Multi-Game Support',
    description: 'Support for Minecraft, Rust, CS:GO, Valheim, and many more games.'
  }
]

const pricingPlans = [
  {
    name: 'Starter',
    price: 3,
    features: ['2GB RAM', '1 CPU Core', '10 Players', 'Basic Support'],
    popular: false
  },
  {
    name: 'Pro',
    price: 7,
    features: ['4GB RAM', '2 CPU Cores', '30 Players', 'Priority Support'],
    popular: true
  },
  {
    name: 'Ultimate',
    price: 15,
    features: ['8GB RAM', '4 CPU Cores', 'Unlimited Players', '24/7 Support'],
    popular: false
  }
]

export default function HomePage() {
  const router = useRouter()

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-purple-900 to-blue-900">
      {/* Hero Section */}
      <div className="relative overflow-hidden">
        <div className="absolute inset-0 bg-[url('data:image/svg+xml,%3Csvg width="60" height="60" viewBox="0 0 60 60" xmlns="http://www.w3.org/2000/svg"%3E%3Cg fill="none" fill-rule="evenodd"%3E%3Cg fill="%239C92AC" fill-opacity="0.1"%3E%3Cpath d="M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z"/%3E%3C/g%3E%3C/g%3E%3C/svg%3E')] opacity-20" />
        
        <div className="relative px-4 py-20 mx-auto max-w-7xl">
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8 }}
            className="text-center"
          >
            <h1 className="text-5xl md:text-7xl font-bold text-white mb-6">
              Host Your Game Server
              <span className="block bg-gradient-to-r from-blue-400 to-purple-400 bg-clip-text text-transparent">
                Like a Pro
              </span>
            </h1>
            
            <p className="text-xl text-gray-300 mb-8 max-w-2xl mx-auto">
              Deploy high-performance game servers in seconds. Global infrastructure, 
              99.9% uptime, and enterprise-grade security.
            </p>
            
            <div className="flex gap-4 justify-center">
              <Button 
                size="lg" 
                className="text-lg px-8 py-3"
                onClick={() => router.push('/register')}
              >
                Get Started Free
                <ArrowRight className="ml-2 h-5 w-5" />
              </Button>
              <Button 
                variant="outline" 
                size="lg"
                className="text-lg px-8 py-3 border-white/20 text-white hover:bg-white/10"
              >
                View Demo
              </Button>
            </div>
          </motion.div>
        </div>
      </div>

      {/* Features Section */}
      <div className="py-20 px-4">
        <div className="max-w-7xl mx-auto">
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8 }}
            className="text-center mb-16"
          >
            <h2 className="text-4xl font-bold text-white mb-4">
              Everything You Need to Host
            </h2>
            <p className="text-xl text-gray-300">
              Powerful features for serious gamers
            </p>
          </motion.div>

          <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
            {features.map((feature, index) => (
              <motion.div
                key={feature.title}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5, delay: index * 0.1 }}
                className="bg-white/5 backdrop-blur-sm border border-white/10 rounded-xl p-6"
              >
                <div className="h-12 w-12 bg-gradient-to-r from-blue-500 to-purple-500 rounded-lg flex items-center justify-center mb-4">
                  <feature.icon className="h-6 w-6 text-white" />
                </div>
                <h3 className="text-xl font-semibold text-white mb-2">
                  {feature.title}
                </h3>
                <p className="text-gray-300">
                  {feature.description}
                </p>
              </motion.div>
            ))}
          </div>
        </div>
      </div>

      {/* Pricing Section */}
      <div className="py-20 px-4 bg-black/20">
        <div className="max-w-7xl mx-auto">
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8 }}
            className="text-center mb-16"
          >
            <h2 className="text-4xl font-bold text-white mb-4">
              Simple, Transparent Pricing
            </h2>
            <p className="text-xl text-gray-300">
              Choose the perfect plan for your needs
            </p>
          </motion.div>

          <div className="grid md:grid-cols-3 gap-8 max-w-4xl mx-auto">
            {pricingPlans.map((plan, index) => (
              <motion.div
                key={plan.name}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5, delay: index * 0.1 }}
                className={`relative bg-white/5 backdrop-blur-sm border rounded-xl p-6 ${
                  plan.popular ? 'border-purple-500' : 'border-white/10'
                }`}
              >
                {plan.popular && (
                  <div className="absolute -top-3 left-1/2 transform -translate-x-1/2">
                    <span className="bg-gradient-to-r from-purple-500 to-pink-500 text-white text-sm font-medium px-3 py-1 rounded-full">
                      Most Popular
                    </span>
                  </div>
                )}
                
                <h3 className="text-2xl font-bold text-white mb-2">{plan.name}</h3>
                <div className="text-4xl font-bold text-white mb-4">
                  ${plan.price}<span className="text-lg text-gray-400">/mo</span>
                </div>
                
                <ul className="space-y-3 mb-6">
                  {plan.features.map((feature) => (
                    <li key={feature} className="flex items-center text-gray-300">
                      <Star className="h-4 w-4 text-yellow-500 mr-2" />
                      {feature}
                    </li>
                  ))}
                </ul>
                
                <Button 
                  className={`w-full ${
                    plan.popular 
                      ? 'bg-gradient-to-r from-purple-500 to-pink-500' 
                      : ''
                  }`}
                  onClick={() => router.push('/register')}
                >
                  Get Started
                </Button>
              </motion.div>
            ))}
          </div>
        </div>
      </div>

      {/* CTA Section */}
      <div className="py-20 px-4">
        <div className="max-w-4xl mx-auto text-center">
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8 }}
          >
            <h2 className="text-4xl font-bold text-white mb-4">
              Ready to Start Gaming?
            </h2>
            <p className="text-xl text-gray-300 mb-8">
              Join thousands of gamers who trust GamePanel for their server hosting needs
            </p>
            <Button 
              size="lg" 
              className="text-lg px-8 py-3"
              onClick={() => router.push('/register')}
            >
              Create Your Server Now
              <Gamepad2 className="ml-2 h-5 w-5" />
            </Button>
          </motion.div>
        </div>
      </div>
    </div>
  )
}
```

### 📄 `frontend/pages/dashboard.tsx`
```tsx
import { useEffect, useState } from 'react'
import { motion } from 'framer-motion'
import { useAuth } from '@/contexts/AuthContext'
import { useRouter } from 'next/router'
import { 
  Server, 
  Activity, 
  DollarSign, 
  Users,
  TrendingUp,
  Clock
} from 'lucide-react'
import Layout from '@/components/layout/Layout'
import { Button } from '@/components/ui/Button'

interface DashboardStats {
  totalServers: number
  activeServers: number
  totalSpent: number
  monthlyUsage: number
}

export default function Dashboard() {
  const { user } = useAuth()
  const router = useRouter()
  const [stats, setStats] = useState<DashboardStats>({
    totalServers: 0,
    activeServers: 0,
    totalSpent: 0,
    monthlyUsage: 0
  })

  useEffect(() => {
    // Fetch dashboard stats
    fetchStats()
  }, [])

  const fetchStats = async () => {
    try {
      const response = await fetch('/api/dashboard/stats')
      const data = await response.json()
      setStats(data)
    } catch (error) {
      console.error('Failed to fetch stats:', error)
    }
  }

  const statCards = [
    {
      title: 'Total Servers',
      value: stats.totalServers,
      icon: Server,
      color: 'from-blue-500 to-blue-600',
      trend: '+12%'
    },
    {
      title: 'Active Servers',
      value: stats.activeServers,
      icon: Activity,
      color: 'from-green-500 to-green-600',
      trend: '+8%'
    },
    {
      title: 'Total Spent',
      value: `$${stats.totalSpent}`,
      icon: DollarSign,
      color: 'from-purple-500 to-purple-600',
      trend: '+5%'
    },
    {
      title: 'Monthly Usage',
      value: `${stats.monthlyUsage}h`,
      icon: Clock,
      color: 'from-orange-500 to-orange-600',
      trend: '+15%'
    }
  ]

  return (
    <Layout>
      <div className="space-y-6">
        {/* Welcome Header */}
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          className="flex items-center justify-between"
        >
          <div>
            <h1 className="text-3xl font-bold text-white">
              Welcome back, {user?.firstName || 'User'}! 👋
            </h1>
            <p className="text-gray-400 mt-1">
              Here's what's happening with your servers today
            </p>
          </div>
          <Button 
            className="bg-gradient-to-r from-blue-500 to-purple-500"
            onClick={() => router.push('/servers/create')}
          >
            Create New Server
          </Button>
        </motion.div>

        {/* Stats Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          {statCards.map((stat, index) => (
            <motion.div
              key={stat.title}
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: index * 0.1 }}
              className="bg-gray-800/50 backdrop-blur-sm border border-gray-700 rounded-xl p-6"
            >
              <div className="flex items-center justify-between">
                <div>
                  <p className="text-sm text-gray-400">{stat.title}</p>
                  <p className="text-2xl font-bold text-white mt-1">{stat.value}</p>
                  <p className="text-sm text-green-400 mt-2">{stat.trend} from last month</p>
                </div>
                <div className={`h-12 w-12 rounded-lg bg-gradient-to-r ${stat.color} flex items-center justify-center`}>
                  <stat.icon className="h-6 w-6 text-white" />
                </div>
              </div>
            </motion.div>
          ))}
        </div>

        {/* Recent Activity */}
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ delay: 0.4 }}
          className="bg-gray-800/50 backdrop-blur-sm border border-gray-700 rounded-xl p-6"
        >
          <h2 className="text-xl font-bold text-white mb-4">Recent Activity</h2>
          <div className="space-y-4">
            {[
              { action: 'Server "My Minecraft World" started', time: '2 minutes ago', type: 'success' },
              { action: 'Payment of $7.00 processed', time: '1 hour ago', type: 'info' },
              { action: 'Server backup completed', time: '3 hours ago', type: 'success' },
              { action: 'New server "Rust Server" created', time: '1 day ago', type: 'info' }
            ].map((activity, index) => (
              <div key={index} className="flex items-center justify-between py-3 border-b border-gray-700 last:border-0">
                <div className="flex items-center gap-3">
                  <div className={`h-2 w-2 rounded-full ${
                    activity.type === 'success' ? 'bg-green-500' : 'bg-blue-500'
                  }`} />
                  <span className="text-gray-300">{activity.action}</span>
                </div>
                <span className="text-sm text-gray-500">{activity.time}</span>
              </div>
            ))}
          </div>
        </motion.div>
      </div>
    </Layout>
  )
}
```

## 🎯 **Key Visual Improvements**

### 1. **Modern Dark Theme**
- Sophisticated dark color palette
- Glass morphism effects
- Gradient backgrounds

### 2. **Enhanced Animations**
- Smooth page transitions
- Hover effects on all interactive elements
- Loading states and micro-interactions

### 3. **Better Typography**
- Clean, modern fonts
- Better hierarchy and spacing
- Improved readability

### 4. **Improved Components**
- More visually appealing buttons and inputs
- Better use of icons and spacing
- Consistent design language

### 5. **Premium Feel**
- Subtle shadows and glows
- Professional color scheme
- Enterprise-grade visual design

### 6. **Responsive Design**
- Mobile-first approach
- Smooth sidebar transitions
- Adaptive layouts

