# Sign-in-up-Auth-Page
Sign in/ Sign up Authorization Page
import React, { useState, useEffect, useRef } from 'react';
import { 
  Smartphone, 
  Mail, 
  Lock, 
  Eye, 
  EyeOff, 
  User, 
  ArrowRight, 
  CheckCircle, 
  LogOut, 
  Sparkles, 
  Music, 
  Play, 
  Pause, 
  Disc, 
  Radio, 
  Wand2, 
  Volume2, 
  VolumeX, 
  Bookmark, 
  History, 
  Sun, 
  Moon, 
  RefreshCw, 
  Sliders, 
  Headphones, 
  Share2, 
  Heart,
  Zap
} from 'lucide-react';

export default function App() {
  // Auth & Navigation States
  const [activeTab, setActiveTab] = useState('signin'); // 'signin' | 'signup'
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [showPassword, setShowPassword] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [toast, setToast] = useState(null);
  const [darkMode, setDarkMode] = useState(true); // Default dark for music-tech feel

  // Auth Form State
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    password: '',
    confirmPassword: '',
  });
  const [errors, setErrors] = useState({});
  const [userProfile, setUserProfile] = useState(null);

  // AI Promptunes Studio State
  const [promptInput, setPromptInput] = useState('');
  const [selectedGenre, setSelectedGenre] = useState('Synthwave');
  const [selectedDuration, setSelectedDuration] = useState('30s');
  const [isGenerating, setIsGenerating] = useState(false);
  const [isPlaying, setIsPlaying] = useState(false);
  const [currentTrack, setCurrentTrack] = useState(null);
  const [savedTracks, setSavedTracks] = useState([
    {
      id: 1,
      title: 'Neon Cyberpunk Pursuit',
      prompt: 'Cyberpunk synthwave upbeat track with driving bassline',
      genre: 'Synthwave',
      duration: '0:30',
      cover: 'from-purple-600 to-indigo-900',
      likes: 124,
      isLiked: true
    },
    {
      id: 2,
      title: 'Midnight Rain Lofi Study',
      prompt: 'Lofi chill study beats with soft rain and vintage vinyl crackle',
      genre: 'Lofi Beat',
      duration: '0:45',
      cover: 'from-emerald-600 to-teal-900',
      likes: 89,
      isLiked: false
    }
  ]);

  const [activeStudioTab, setActiveStudioTab] = useState('create'); // 'create' | 'library'

  // Pre-configured Prompt Suggestions
  const samplePrompts = [
    "Lofi chill study beats with rain",
    "Cyberpunk synthwave upbeat track",
    "Epic cinematic orchestral battle theme",
    "Acoustic indie folk sunset melody"
  ];

  const genres = ['Synthwave', 'Lofi Beat', 'Cinematic', 'Indie Folk', 'EDM Drop', 'Ambient Wave'];

  // Toast Helper
  const showToast = (message, type = 'success') => {
    setToast({ message, type });
    setTimeout(() => {
      setToast(null);
    }, 3200);
  };

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: null }));
    }
  };

  const validateForm = () => {
    const newErrors = {};
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

    if (!formData.email.trim()) {
      newErrors.email = 'Email address is required';
    } else if (!emailRegex.test(formData.email)) {
      newErrors.email = 'Enter a valid email address';
    }

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    if (activeTab === 'signup') {
      if (!formData.name.trim()) {
        newErrors.name = 'Full name is required';
      }
      if (formData.password !== formData.confirmPassword) {
        newErrors.confirmPassword = 'Passwords do not match';
      }
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!validateForm()) return;

    setIsLoading(true);

    setTimeout(() => {
      setIsLoading(false);
      setIsAuthenticated(true);
      setUserProfile({
        name: activeTab === 'signup' ? formData.name : 'Aria Vance',
        email: formData.email,
        avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&q=80&w=250',
        method: 'Email & Password',
        credits: 50
      });

      showToast(
        activeTab === 'signin' 
          ? 'Welcome back to Promptunes!' 
          : 'Welcome to AI Promptunes Studio!'
      );
    }, 1400);
  };

  const handleGoogleSignIn = () => {
    setIsLoading(true);

    setTimeout(() => {
      setIsLoading(false);
      setIsAuthenticated(true);
      setUserProfile({
        name: 'Devon Sparks',
        email: 'devon.sparks@gmail.com',
        avatar: 'https://images.unsplash.com/photo-1539571696357-5a69c17a67c6?auto=format&fit=crop&q=80&w=250',
        method: 'Google Account',
        credits: 50
      });

      showToast('Authenticated with Google!');
    }, 1200);
  };

  const handleLogout = () => {
    setIsAuthenticated(false);
    setIsPlaying(false);
    setCurrentTrack(null);
    setFormData({ name: '', email: '', password: '', confirmPassword: '' });
    setErrors({});
    showToast('Signed out of Promptunes', 'info');
  };

  // AI Music Generation Logic
  const handleGenerateMusic = () => {
    if (!promptInput.trim()) {
      showToast('Please enter a prompt to generate music!', 'info');
      return;
    }

    setIsGenerating(true);
    setIsPlaying(false);

    setTimeout(() => {
      setIsGenerating(false);

      const colorGradients = [
        'from-fuchsia-600 to-pink-900',
        'from-cyan-600 to-blue-900',
        'from-violet-600 to-purple-900',
        'from-amber-500 to-rose-900'
      ];
      const randomGradient = colorGradients[Math.floor(Math.random() * colorGradients.length)];

      const newTrack = {
        id: Date.now(),
        title: promptInput.length > 22 ? promptInput.substring(0, 22) + '...' : promptInput,
        prompt: promptInput,
        genre: selectedGenre,
        duration: selectedDuration === '30s' ? '0:30' : '1:00',
        cover: randomGradient,
        likes: 1,
        isLiked: false
      };

      setCurrentTrack(newTrack);
      setSavedTracks(prev => [newTrack, ...prev]);
      setIsPlaying(true);
      
      // Deduct credit simulation
      setUserProfile(prev => prev ? { ...prev, credits: Math.max(0, prev.credits - 2) } : null);

      showToast('AI Tune generated successfully!');
    }, 2800);
  };

  const toggleLikeTrack = (id) => {
    setSavedTracks(prev =>
      prev.map(t => {
        if (t.id === id) {
          const isLiked = !t.isLiked;
          return {
            ...t,
            isLiked,
            likes: isLiked ? t.likes + 1 : t.likes - 1
          };
        }
        return t;
      })
    );
  };

  return (
    <div className={`min-h-screen ${darkMode ? 'bg-slate-950 text-slate-100' : 'bg-slate-100 text-slate-800'} transition-colors duration-300 flex flex-col justify-between items-center p-3 sm:p-6 font-sans`}>
      
      {/* App Header Bar */}
      <header className="w-full max-w-4xl flex justify-between items-center mb-4 sm:mb-6">
        <div className="flex items-center space-x-2.5">
          <div className="bg-gradient-to-tr from-fuchsia-600 to-violet-600 text-white p-2.5 rounded-2xl shadow-lg shadow-fuchsia-500/20 flex items-center justify-center">
            <Music className="w-6 h-6 animate-pulse" />
          </div>
          <div>
            <div className="flex items-center gap-1.5">
              <h1 className="text-xl font-extrabold tracking-tight bg-gradient-to-r from-fuchsia-500 via-purple-400 to-cyan-400 bg-clip-text text-transparent">
                Promptunes
              </h1>
              <span className="px-1.5 py-0.5 text-[9px] font-extrabold bg-fuchsia-500/20 text-fuchsia-400 border border-fuchsia-500/30 rounded-md uppercase tracking-wider">AI Studio</span>
            </div>
            <p className="text-xs text-slate-400">Prompt-to-Music Mobile Simulator</p>
          </div>
        </div>

        <button
          onClick={() => setDarkMode(!darkMode)}
          className={`p-2.5 rounded-full ${darkMode ? 'bg-slate-800/80 text-amber-400 border border-slate-700' : 'bg-white text-slate-700 shadow-sm'} transition-all hover:scale-105`}
          title="Toggle Mobile Simulator Theme"
        >
          {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />}
        </button>
      </header>

      {/* Main Container - Mobile Frame */}
      <div className="relative w-full max-w-xs sm:max-w-sm my-auto">
        
        {/* Floating Toast Message */}
        {toast && (
          <div className={`absolute -top-12 left-1/2 -translate-x-1/2 z-50 w-11/12 flex items-center gap-2 px-3.5 py-2.5 rounded-xl shadow-2xl text-xs font-semibold text-white transition-all transform animate-bounce ${
            toast.type === 'info' ? 'bg-slate-800 border border-slate-700' : 'bg-gradient-to-r from-fuchsia-600 to-indigo-600'
          }`}>
            <Sparkles className="w-4 h-4 shrink-0 text-amber-300" />
            <span className="flex-1 truncate">{toast.message}</span>
          </div>
        )}

        {/* Smartphone Shell Frame */}
        <div className={`relative rounded-[44px] border-[10px] ${darkMode ? 'border-slate-800 bg-slate-900' : 'border-slate-800 bg-slate-900'} shadow-2xl overflow-hidden transition-all duration-300 min-h-[690px] flex flex-col justify-between`}>
          
          {/* Top Speaker Notch & Dynamic Status Bar */}
          <div className="w-full bg-slate-900 px-6 pt-3 pb-2 flex justify-between items-center text-[10px] font-semibold text-slate-400 select-none z-20">
            <span>9:41</span>
            <div className="w-20 h-4 bg-black rounded-full mx-auto flex items-center justify-center gap-1.5">
              <div className="w-2 h-2 bg-fuchsia-500 rounded-full animate-ping"></div>
              <div className="w-2 h-2 bg-cyan-400 rounded-full"></div>
            </div>
            <div className="flex items-center space-x-1.5 text-[9px]">
              <span>5G</span>
              <div className="w-4 h-2 border border-slate-500 rounded-xs p-0.5 flex items-center">
                <div className="w-full h-full bg-emerald-400 rounded-xs"></div>
              </div>
            </div>
          </div>

          {/* Screen Content Scrollable Container */}
          <div className="flex-1 px-5 py-3 flex flex-col overflow-y-auto text-slate-100">
            
            {!isAuthenticated ? (
              /* AUTHENTICATION VIEW */
              <div className="flex-1 flex flex-col justify-between py-2">
                
                {/* AI Music Hero Header */}
                <div className="text-center my-2">
                  <div className="relative inline-flex p-3 rounded-2xl bg-gradient-to-br from-fuchsia-500/20 to-cyan-500/20 border border-fuchsia-500/30 text-fuchsia-400 mb-2 group">
                    <Music className="w-7 h-7 text-fuchsia-400" />
                    <Sparkles className="w-4 h-4 text-cyan-300 absolute -top-1 -right-1 animate-spin" />
                  </div>
                  <h2 className="text-2xl font-black tracking-tight text-white">
                    AI Promptunes
                  </h2>
                  <p className="text-[11px] text-fuchsia-300/80 font-medium mt-0.5">
                    Turn ideas into original AI music
                  </p>

                  {/* Feature Prompt Pills Showcase */}
                  <div className="mt-3 py-1.5 px-3 bg-slate-800/80 rounded-xl border border-slate-700/60 text-left">
                    <div className="flex items-center gap-1.5 text-[10px] text-cyan-400 font-bold uppercase mb-1">
                      <Zap className="w-3 h-3" /> Live Prompt Preview
                    </div>
                    <p className="text-[10px] text-slate-300 italic truncate">
                      "Cyberpunk synthwave upbeat track with driving bassline"
                    </p>
                  </div>
                </div>

                {/* Sign-In / Sign-Up Tabs */}
                <div className="p-1 bg-slate-800/90 rounded-xl flex my-3 border border-slate-700/60">
                  <button
                    onClick={() => { setActiveTab('signin'); setErrors({}); }}
                    className={`flex-1 py-1.5 text-xs font-bold rounded-lg transition-all duration-200 ${
                      activeTab === 'signin'
                        ? 'bg-gradient-to-r from-fuchsia-600 to-indigo-600 text-white shadow-md'
                        : 'text-slate-400 hover:text-white'
                    }`}
                  >
                    Sign In
                  </button>
                  <button
                    onClick={() => { setActiveTab('signup'); setErrors({}); }}
                    className={`flex-1 py-1.5 text-xs font-bold rounded-lg transition-all duration-200 ${
                      activeTab === 'signup'
                        ? 'bg-gradient-to-r from-fuchsia-600 to-indigo-600 text-white shadow-md'
                        : 'text-slate-400 hover:text-white'
                    }`}
                  >
                    Sign Up
                  </button>
                </div>

                {/* Google Sign-In Option */}
                <button
                  type="button"
                  onClick={handleGoogleSignIn}
                  disabled={isLoading}
                  className="w-full py-2.5 px-4 border border-slate-700 rounded-xl flex items-center justify-center space-x-2.5 text-xs font-semibold text-slate-200 bg-slate-800 hover:bg-slate-750 active:scale-[0.98] transition-all disabled:opacity-50 shadow-sm"
                >
                  <svg className="w-4 h-4 shrink-0" viewBox="0 0 24 24">
                    <path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" />
                    <path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" />
                    <path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.06H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.94l2.85-2.22.81-.63z" />
                    <path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.06l3.66 2.84c.87-2.6 3.3-4.52 6.16-4.52z" />
                  </svg>
                  <span>Continue with Google</span>
                </button>

                {/* Divider Line */}
                <div className="relative my-3">
                  <div className="absolute inset-0 flex items-center">
                    <div className="w-full border-t border-slate-800"></div>
                  </div>
                  <div className="relative flex justify-center text-[9px] uppercase tracking-wider">
                    <span className="bg-slate-900 px-2 text-slate-500">or sign in with email</span>
                  </div>
                </div>

                {/* Authentication Form */}
                <form onSubmit={handleSubmit} className="space-y-2.5">
                  {activeTab === 'signup' && (
                    <div>
                      <label className="block text-[10px] font-semibold text-slate-400 mb-1">
                        Artist / Producer Name
                      </label>
                      <div className="relative">
                        <User className="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-500" />
                        <input
                          type="text"
                          name="name"
                          value={formData.name}
                          onChange={handleInputChange}
                          placeholder="Aria Vance"
                          className={`w-full pl-9 pr-3 py-2 bg-slate-800/90 border ${
                            errors.name ? 'border-red-500' : 'border-slate-700/80'
                          } rounded-xl text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-fuchsia-500/40 transition-all`}
                        />
                      </div>
                      {errors.name && <p className="text-[10px] text-red-400 mt-0.5">{errors.name}</p>}
                    </div>
                  )}

                  <div>
                    <label className="block text-[10px] font-semibold text-slate-400 mb-1">
                      Email Address
                    </label>
                    <div className="relative">
                      <Mail className="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-500" />
                      <input
                        type="email"
                        name="email"
                        value={formData.email}
                        onChange={handleInputChange}
                        placeholder="artist@promptunes.ai"
                        className={`w-full pl-9 pr-3 py-2 bg-slate-800/90 border ${
                          errors.email ? 'border-red-500' : 'border-slate-700/80'
                        } rounded-xl text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-fuchsia-500/40 transition-all`}
                      />
                    </div>
                    {errors.email && <p className="text-[10px] text-red-400 mt-0.5">{errors.email}</p>}
                  </div>

                  <div>
                    <div className="flex justify-between items-center mb-1">
                      <label className="block text-[10px] font-semibold text-slate-400">
                        Password
                      </label>
                      {activeTab === 'signin' && (
                        <button
                          type="button"
                          onClick={() => showToast('Password reset link sent to email!', 'info')}
                          className="text-[10px] font-medium text-fuchsia-400 hover:underline"
                        >
                          Forgot?
                        </button>
                      )}
                    </div>
                    <div className="relative">
                      <Lock className="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-500" />
                      <input
                        type={showPassword ? 'text' : 'password'}
                        name="password"
                        value={formData.password}
                        onChange={handleInputChange}
                        placeholder="••••••••"
                        className={`w-full pl-9 pr-9 py-2 bg-slate-800/90 border ${
                          errors.password ? 'border-red-500' : 'border-slate-700/80'
                        } rounded-xl text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-fuchsia-500/40 transition-all`}
                      />
                      <button
                        type="button"
                        onClick={() => setShowPassword(!showPassword)}
                        className="absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300"
                      >
                        {showPassword ? <EyeOff className="w-4 h-4" /> : <Eye className="w-4 h-4" />}
                      </button>
                    </div>
                    {errors.password && <p className="text-[10px] text-red-400 mt-0.5">{errors.password}</p>}
                  </div>

                  {activeTab === 'signup' && (
                    <div>
                      <label className="block text-[10px] font-semibold text-slate-400 mb-1">
                        Confirm Password
                      </label>
                      <div className="relative">
                        <Lock className="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-500" />
                        <input
                          type={showPassword ? 'text' : 'password'}
                          name="confirmPassword"
                          value={formData.confirmPassword}
                          onChange={handleInputChange}
                          placeholder="••••••••"
                          className={`w-full pl-9 pr-3 py-2 bg-slate-800/90 border ${
                            errors.confirmPassword ? 'border-red-500' : 'border-slate-700/80'
                          } rounded-xl text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-fuchsia-500/40 transition-all`}
                        />
                      </div>
                      {errors.confirmPassword && <p className="text-[10px] text-red-400 mt-0.5">{errors.confirmPassword}</p>}
                    </div>
                  )}

                  <button
                    type="submit"
                    disabled={isLoading}
                    className="w-full mt-2 py-2.5 px-4 bg-gradient-to-r from-fuchsia-600 via-purple-600 to-indigo-600 hover:brightness-110 active:scale-[0.98] text-white rounded-xl text-xs font-bold shadow-lg shadow-fuchsia-500/25 transition-all flex items-center justify-center space-x-2 disabled:opacity-70"
                  >
                    {isLoading ? (
                      <RefreshCw className="w-4 h-4 animate-spin" />
                    ) : (
                      <>
                        <span>{activeTab === 'signin' ? 'Enter AI Studio' : 'Start Creating Free'}</span>
                        <ArrowRight className="w-4 h-4" />
                      </>
                    )}
                  </button>
                </form>
              </div>
            ) : (
              /* POST-AUTHENTICATION STUDIO VIEW */
              <div className="flex-1 flex flex-col justify-between py-1">
                
                {/* User Header Bar */}
                <div className="flex items-center justify-between pb-2 border-b border-slate-800">
                  <div className="flex items-center gap-2">
                    <img
                      src={userProfile?.avatar}
                      alt="User"
                      className="w-8 h-8 rounded-full border border-fuchsia-500/50 object-cover"
                    />
                    <div>
                      <h4 className="text-xs font-bold text-white truncate max-w-[110px]">{userProfile?.name}</h4>
                      <div className="flex items-center gap-1">
                        <Sparkles className="w-2.5 h-2.5 text-amber-400" />
                        <span className="text-[9px] text-slate-400">{userProfile?.credits} AI Credits</span>
                      </div>
                    </div>
                  </div>

                  <button
                    onClick={handleLogout}
                    className="p-1.5 text-slate-400 hover:text-red-400 hover:bg-slate-800 rounded-lg transition-all"
                    title="Sign Out"
                  >
                    <LogOut className="w-4 h-4" />
                  </button>
                </div>

                {/* Studio Section Tabs */}
                <div className="flex border-b border-slate-800 my-2">
                  <button
                    onClick={() => setActiveStudioTab('create')}
                    className={`flex-1 py-1.5 text-[11px] font-bold border-b-2 transition-all flex items-center justify-center gap-1.5 ${
                      activeStudioTab === 'create'
                        ? 'border-fuchsia-500 text-fuchsia-400'
                        : 'border-transparent text-slate-400 hover:text-slate-200'
                    }`}
                  >
                    <Wand2 className="w-3.5 h-3.5" /> Generator
                  </button>
                  <button
                    onClick={() => setActiveStudioTab('library')}
                    className={`flex-1 py-1.5 text-[11px] font-bold border-b-2 transition-all flex items-center justify-center gap-1.5 ${
                      activeStudioTab === 'library'
                        ? 'border-fuchsia-500 text-fuchsia-400'
                        : 'border-transparent text-slate-400 hover:text-slate-200'
                    }`}
                  >
                    <Headphones className="w-3.5 h-3.5" /> My Tunes ({savedTracks.length})
                  </button>
                </div>

                {/* MAIN STUDIO GENERATOR TAB */}
                {activeStudioTab === 'create' && (
                  <div className="flex-1 flex flex-col justify-between space-y-3">
                    
                    {/* Audio Player Card (Active or Simulated) */}
                    <div className={`p-3 rounded-2xl bg-gradient-to-b ${currentTrack ? currentTrack.cover : 'from-slate-800 to-slate-900'} border border-slate-700/80 shadow-lg relative overflow-hidden transition-all`}>
                      <div className="flex items-center gap-3">
                        {/* Animated Vinyl Disc */}
                        <div className={`w-12 h-12 rounded-full bg-slate-950 flex items-center justify-center shrink-0 border border-slate-700 ${isPlaying ? 'animate-spin' : ''}`} style={{ animationDuration: '6s' }}>
                          <Disc className="w-6 h-6 text-fuchsia-400" />
                        </div>

                        <div className="flex-1 min-w-0">
                          <span className="text-[9px] uppercase font-bold text-cyan-400 tracking-wider">
                            {currentTrack ? currentTrack.genre : 'AI Player'}
                          </span>
                          <h4 className="text-xs font-bold text-white truncate">
                            {currentTrack ? currentTrack.title : 'Ready to compose...'}
                          </h4>
                          <p className="text-[10px] text-slate-300 truncate">
                            {currentTrack ? currentTrack.prompt : 'Enter prompt below to generate track'}
                          </p>
                        </div>

                        {currentTrack && (
                          <button
                            onClick={() => setIsPlaying(!isPlaying)}
                            className="w-9 h-9 rounded-full bg-fuchsia-500 text-white flex items-center justify-center shadow-md hover:scale-105 active:scale-95 transition-all shrink-0"
                          >
                            {isPlaying ? <Pause className="w-4 h-4" /> : <Play className="w-4 h-4 ml-0.5" />}
                          </button>
                        )}
                      </div>

                      {/* Equalizer Audio Visualizer Effect */}
                      <div className="mt-3 pt-2 border-t border-white/10 flex items-end justify-between h-7 px-1 gap-1">
                        {[40, 75, 30, 90, 60, 100, 45, 80, 50, 95, 35, 70, 85, 40, 60].map((height, i) => (
                          <div
                            key={i}
                            className={`w-1 bg-gradient-to-t from-fuchsia-500 to-cyan-400 rounded-full transition-all duration-300 ${
                              isPlaying ? 'animate-pulse' : 'opacity-40'
                            }`}
                            style={{
                              height: isPlaying ? `${Math.max(15, (height * (i % 2 === 0 ? 0.9 : 0.6)))}%` : '20%',
                              animationDelay: `${i * 0.1}s`
                            }}
                          />
                        ))}
                      </div>
                    </div>

                    {/* AI Prompt Input Form */}
                    <div className="space-y-2">
                      <label className="block text-[10px] font-bold text-slate-300 uppercase tracking-wider flex items-center gap-1">
                        <Sparkles className="w-3 h-3 text-fuchsia-400" /> Describe Your Music Prompt
                      </label>
                      
                      <div className="relative">
                        <textarea
                          rows={2}
                          value={promptInput}
                          onChange={(e) => setPromptInput(e.target.value)}
                          placeholder="e.g. Upbeat cyberpunk synthwave with energetic bass..."
                          className="w-full p-2.5 bg-slate-800/90 border border-slate-700 rounded-xl text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-fuchsia-500/40 resize-none"
                        />
                      </div>

                      {/* Quick Prompt Chips */}
                      <div className="flex items-center gap-1.5 overflow-x-auto pb-1 text-[9px] no-scrollbar">
                        <span className="text-slate-500 shrink-0 font-medium">Try:</span>
                        {samplePrompts.map((p, idx) => (
                          <button
                            key={idx}
                            onClick={() => setPromptInput(p)}
                            className="px-2 py-1 bg-slate-800 hover:bg-slate-700 border border-slate-700/80 text-slate-300 rounded-lg shrink-0 transition-all truncate max-w-[130px]"
                          >
                            {p}
                          </button>
                        ))}
                      </div>
                    </div>

                    {/* Genre & Settings */}
                    <div className="grid grid-cols-2 gap-2">
                      <div>
                        <label className="block text-[9px] font-bold text-slate-400 mb-1">Genre Mood</label>
                        <select
                          value={selectedGenre}
                          onChange={(e) => setSelectedGenre(e.target.value)}
                          className="w-full p-2 bg-slate-800 border border-slate-700 rounded-xl text-xs text-slate-200 focus:outline-none focus:ring-1 focus:ring-fuchsia-500"
                        >
                          {genres.map((g) => (
                            <option key={g} value={g}>{g}</option>
                          ))}
                        </select>
                      </div>

                      <div>
                        <label className="block text-[9px] font-bold text-slate-400 mb-1">Track Duration</label>
                        <div className="flex bg-slate-800 p-0.5 border border-slate-700 rounded-xl">
                          <button
                            onClick={() => setSelectedDuration('30s')}
                            className={`flex-1 py-1.5 text-[10px] font-bold rounded-lg transition-all ${
                              selectedDuration === '30s' ? 'bg-fuchsia-600 text-white' : 'text-slate-400'
                            }`}
                          >
                            30 Sec
                          </button>
                          <button
                            onClick={() => setSelectedDuration('60s')}
                            className={`flex-1 py-1.5 text-[10px] font-bold rounded-lg transition-all ${
                              selectedDuration === '60s' ? 'bg-fuchsia-600 text-white' : 'text-slate-400'
                            }`}
                          >
                            60 Sec
                          </button>
                        </div>
                      </div>
                    </div>

                    {/* Generate Tune Action Button */}
                    <button
                      onClick={handleGenerateMusic}
                      disabled={isGenerating}
                      className="w-full py-3 px-4 bg-gradient-to-r from-fuchsia-600 via-purple-600 to-cyan-600 hover:brightness-110 active:scale-[0.98] text-white rounded-xl text-xs font-extrabold shadow-lg shadow-fuchsia-500/25 transition-all flex items-center justify-center space-x-2 disabled:opacity-70"
                    >
                      {isGenerating ? (
                        <>
                          <RefreshCw className="w-4 h-4 animate-spin" />
                          <span>AI Composing Music...</span>
                        </>
                      ) : (
                        <>
                          <Wand2 className="w-4 h-4" />
                          <span>Generate Tune (2 Credits)</span>
                        </>
                      )}
                    </button>

                  </div>
                )}

                {/* MY TUNE LIBRARY TAB */}
                {activeStudioTab === 'library' && (
                  <div className="flex-1 flex flex-col space-y-2 overflow-y-auto max-h-[360px] pr-1">
                    <h5 className="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Your Generated Tracks</h5>
                    
                    {savedTracks.map((track) => (
                      <div
                        key={track.id}
                        className="p-2.5 bg-slate-800/80 border border-slate-700/80 rounded-xl flex items-center justify-between gap-2 hover:bg-slate-800 transition-all"
                      >
                        <div
                          onClick={() => {
                            setCurrentTrack(track);
                            setIsPlaying(true);
                          }}
                          className="flex items-center gap-2.5 flex-1 min-w-0 cursor-pointer"
                        >
                          <div className={`w-9 h-9 rounded-lg bg-gradient-to-br ${track.cover} flex items-center justify-center shrink-0 shadow-sm`}>
                            <Music className="w-4 h-4 text-white" />
                          </div>
                          <div className="min-w-0">
                            <h5 className="text-xs font-bold text-white truncate">{track.title}</h5>
                            <p className="text-[9px] text-slate-400">{track.genre} • {track.duration}</p>
                          </div>
                        </div>

                        <div className="flex items-center gap-1">
                          <button
                            onClick={() => toggleLikeTrack(track.id)}
                            className={`p-1.5 rounded-lg transition-all ${
                              track.isLiked ? 'text-rose-500 bg-rose-500/10' : 'text-slate-400 hover:text-slate-200'
                            }`}
                          >
                            <Heart className={`w-3.5 h-3.5 ${track.isLiked ? 'fill-rose-500' : ''}`} />
                          </button>
                          <button
                            onClick={() => {
                              setCurrentTrack(track);
                              setIsPlaying(true);
                            }}
                            className="p-1.5 text-fuchsia-400 hover:bg-fuchsia-500/10 rounded-lg"
                          >
                            <Play className="w-3.5 h-3.5" />
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                )}

              </div>
            )}

          </div>

          {/* Bottom Mobile Navigation / Home Bar */}
          <div className="w-full pb-2 pt-1 flex justify-center bg-slate-900">
            <div className="w-28 h-1 bg-slate-700 rounded-full"></div>
          </div>

        </div>
      </div>

      {/* Footer Info */}
      <footer className="mt-4 text-center text-xs text-slate-500">
        <p>AI Promptunes Mobile Auth & Studio Demo • Built with React & Tailwind</p>
      </footer>

    </div>
  );
}
