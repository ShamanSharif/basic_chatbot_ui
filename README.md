# Jcena AI Chatbot

A modern, responsive chatbot interface built with SvelteKit and Tailwind CSS. This application provides an intuitive chat experience with real-time messaging capabilities, customizable themes, and a beautiful user interface.

## 🚀 Features

### Core Functionality

- **Real-time Chat Interface**: WebSocket-based messaging for instant communication
- **Dual Interface Modes**:
  - Landing page with hero section and quick access buttons
  - Full conversation view with message history
- **Message Persistence**: Chat history maintained during session
- **Responsive Design**: Optimized for desktop and mobile devices

### User Experience

- **Theme Customization**: Toggle between day and night modes
- **Font Size Options**: Adjustable text size (small, medium, large, system)
- **Settings Persistence**: User preferences saved in localStorage
- **Modern UI**: Clean, intuitive interface with smooth animations

### Technical Features

- **SvelteKit Framework**: Modern full-stack web framework
- **TypeScript Support**: Full type safety and better development experience
- **Tailwind CSS**: Utility-first CSS framework for rapid styling
- **WebSocket Integration**: Real-time bidirectional communication
- **Component Architecture**: Modular, reusable components

## 📱 Screenshots

### Landing Page

![Landing Page](doc/Screenshot%202025-07-16%20at%2002-13-56%20.png)

### Conversation Interface

![Conversation View](doc/Screenshot%202025-07-16%20at%2002-14-07%20.png)

## 🏗️ Project Structure

```
chatbot_svelte/
├── src/
│   ├── lib/components/          # Reusable UI components
│   │   ├── BigPhatButton.svelte # Main action button
│   │   ├── Hero.svelte          # Landing page hero section
│   │   ├── Navigation.svelte    # Top navigation bar
│   │   ├── Suggestion.svelte    # Quick suggestion chips
│   │   └── TextInput.svelte     # Message input component
│   ├── routes/                  # SvelteKit routing
│   │   ├── +layout.svelte       # Root layout
│   │   ├── +page.svelte         # Landing page
│   │   └── conversation/        # Chat interface
│   │       └── +page.svelte     # Full conversation view
│   └── app.html                 # HTML template
├── static/assets/               # Static assets
│   ├── chatbot.svg              # Chatbot icon
│   └── app-icon.svg             # Application icon
└── doc/                         # Documentation and screenshots
```

## 🛠️ Technology Stack

- **Frontend Framework**: SvelteKit 2.22.0
- **Styling**: Tailwind CSS 4.0.0
- **Language**: TypeScript 5.0.0
- **Build Tool**: Vite 7.0.4
- **Package Manager**: pnpm
- **Code Quality**: ESLint, Prettier
- **Real-time Communication**: WebSocket

## 🚀 Getting Started

### Prerequisites

- Node.js (version 18 or higher)
- pnpm (recommended) or npm

### Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd chatbot_svelte
   ```

2. **Install dependencies**

   ```bash
   pnpm install
   ```

3. **Start development server**

   ```bash
   pnpm dev
   ```

4. **Open in browser**
   Navigate to `http://localhost:5173` to view the application

### Development Commands

```bash
# Start development server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview

# Run type checking
pnpm check

# Format code
pnpm format

# Lint code
pnpm lint
```

## 🔧 Configuration

### WebSocket Backend

The application is configured to connect to a WebSocket backend. Update the endpoint in the following files:

- `src/routes/+page.svelte` (line 32)
- `src/routes/conversation/+page.svelte` (line 32)

Replace `'wss://your-chatbot-endpoint.com'` with your actual WebSocket server URL.

### Environment Variables

Create a `.env` file in the root directory for environment-specific configuration:

```env
VITE_WEBSOCKET_URL=wss://your-chatbot-endpoint.com
VITE_API_BASE_URL=https://your-api-endpoint.com
```

## 🎨 Customization

### Themes

The application supports multiple theme modes:

- **Default**: Clean, modern interface
- **Night**: Dark mode for reduced eye strain
- **Day**: Light mode for bright environments

### Font Sizes

Users can adjust text size through the navigation menu:

- Small: Compact text
- Medium: Standard size
- Large: Enhanced readability
- System: Follows browser/system preferences

## 📦 Deployment

### Build for Production

```bash
pnpm build
```

### Deploy Options

- **Vercel**: Zero-config deployment for SvelteKit
- **Netlify**: Automatic builds and deployments
- **Docker**: Containerized deployment
- **Static Hosting**: Deploy the built static files

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For support and questions:

- Create an issue in the repository
- Check the documentation
- Review the code comments for implementation details

---

**Built with ❤️ using SvelteKit and Tailwind CSS**
