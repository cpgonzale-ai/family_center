import React, { useState, useEffect, useRef } from 'react';
import { 
  ShoppingCart, 
  Search, 
  MapPin, 
  Package, 
  Truck, 
  Clock, 
  CheckCircle, 
  XCircle, 
  Menu, 
  User, 
  LogOut,
  TrendingUp,
  AlertTriangle,
  Smartphone, 
  Printer,
  Lock,
  ChevronRight,
  ChevronDown,
  ChevronLeft, 
  Info,
  Store,
  Check,
  FileText,
  List,
  ArrowLeft,
  Copy,
  Filter,
  Gamepad2,
  Watch,
  Headphones,
  Laptop,
  Tablet,
  Share2,        
  MessageCircle,
  Calendar,
  DollarSign,
  Phone,
  Trash2,
  Plus,
  Minus,
  Map,
  Crosshair,
  Navigation,
  CreditCard,
  Receipt,
  Box
} from 'lucide-react';

// --- DATA STRUCTURES (Graph/Tree) ---

const ZONES = [
  { id: 1, name: 'Asunción (Centro)', price: 20000, cities: ['Asunción'] },
  { id: 2, name: 'Asunción (Villamorra)', price: 25000, cities: ['Asunción'] },
  { id: 3, name: 'San Lorenzo', price: 30000, cities: ['San Lorenzo'] },
  { id: 4, name: 'Luque', price: 30000, cities: ['Luque'] },
  { id: 5, name: 'Fernando de la Mora', price: 25000, cities: ['Fernando de la Mora'] },
  { id: 6, name: 'Lambaré', price: 25000, cities: ['Lambaré'] },
];

const STORES = [
  { id: 'central', name: 'Casa Central', address: 'Av. Principal 123' },
  { id: 'capuchi', name: 'Sucursal Capuchi', address: 'Calle Capuchi 456' },
  { id: 'family', name: 'Family Center', address: 'Shopping Family' }
];

const CATEGORY_HIERARCHY = [
  { 
    id: 'root_all', 
    label: 'Todas las Categorías', 
    value: 'Todos', 
    type: 'leaf',
    icon: List 
  },
  { 
    id: 'cat_tech', 
    label: 'Tecnología & Móviles', 
    type: 'parent', 
    icon: Smartphone,
    children: [
      { id: 'sub_cel', label: 'Celulares', value: 'Celulares', type: 'leaf' },
      { id: 'sub_acc', label: 'Accesorios', value: 'Accesorios', type: 'leaf' }, 
      { id: 'sub_tab', label: 'Tablets', value: 'Tablets', type: 'leaf' }        
    ]
  },
  { 
    id: 'cat_wearables', 
    label: 'Wearables & Smartwatch', 
    type: 'parent', 
    icon: Watch,
    children: [
      { id: 'sub_wear', label: 'Smartwatches', value: 'Wearables', type: 'leaf' },
      { id: 'sub_bands', label: 'Smart Bands', value: 'Smart Bands', type: 'leaf' }
    ]
  },
  { 
    id: 'cat_entertainment', 
    label: 'Entretenimiento', 
    type: 'parent', 
    icon: Gamepad2,
    children: [
      { id: 'sub_audio', label: 'Audio & Sonido', value: 'Audio', type: 'leaf' },
      { id: 'sub_consolas', label: 'Consolas & Videojuegos', value: 'Consolas', type: 'leaf' },
      { id: 'sub_tv', label: 'Televisores', value: 'TV', type: 'leaf' }
    ]
  }
];

const PRODUCTS = [
  { 
    id: 101, 
    name: 'Samsung Galaxy A26', 
    price: 1570000, 
    image: 'samsung', 
    category: 'Celulares',
    stock: { central: 2, capuchi: 5, family: 0 },
    description: 'Pantalla Super AMOLED de 6.5", Procesador Octa-Core, Batería de 5000mAh con carga rápida.',
    features: ['128GB Almacenamiento', '6GB RAM', 'Cámara 50MP']
  },
  { 
    id: 102, 
    name: 'Poco X7 Pro 256GB', 
    price: 2850000, 
    image: 'poco', 
    category: 'Celulares',
    stock: { central: 0, capuchi: 1, family: 0 },
    description: 'Rendimiento extremo con Snapdragon 8 Gen 2. Pantalla 120Hz para gaming fluido.',
    features: ['256GB UFS 3.1', '8GB RAM', 'Refrigeración Líquida']
  },
  { 
    id: 103, 
    name: 'JBL Flip 6', 
    price: 850000, 
    image: 'jbl', 
    category: 'Audio',
    stock: { central: 10, capuchi: 8, family: 4 },
    description: 'Altavoz portátil resistente al agua IP67. Hasta 12 horas de reproducción.',
    features: ['Bluetooth 5.1', 'PartyBoost', 'Graves profundos']
  },
  { 
    id: 104, 
    name: 'Xiaomi Redmi Note 13', 
    price: 1450000, 
    image: 'xiaomi', 
    category: 'Celulares',
    stock: { central: 5, capuchi: 0, family: 2 },
    description: 'El rey de la gama media. Cámara de 108MP y carga turbo de 33W.',
    features: ['128GB Almacenamiento', 'Pantalla AMOLED', 'Dual SIM']
  },
  { 
    id: 105, 
    name: 'PlayStation 5 Slim', 
    price: 4500000, 
    image: 'ps5', 
    category: 'Consolas',
    stock: { central: 2, capuchi: 2, family: 1 },
    description: 'Consola de última generación. Diseño slim con lector de discos.',
    features: ['4K 120Hz', 'SSD 1TB', 'DualSense']
  },
  { 
    id: 106, 
    name: 'Apple Watch SE 2', 
    price: 2100000, 
    image: 'watch', 
    category: 'Wearables',
    stock: { central: 4, capuchi: 0, family: 0 },
    description: 'El compañero ideal para tu iPhone. Monitoreo de actividad y salud.',
    features: ['GPS', 'Resistente al agua', 'Detección de accidentes']
  }
];

// --- HELPERS ---

const getStatusColor = (status) => {
  switch(status) {
    case 'pending': return 'bg-orange-100 text-orange-700 border-orange-200';
    case 'approved': return 'bg-blue-100 text-blue-700 border-blue-200';
    case 'delivering': return 'bg-indigo-100 text-indigo-700 border-indigo-200';
    case 'delivered': return 'bg-green-100 text-green-700 border-green-200';
    case 'cancelled': 
    case 'cancelled_timeout': return 'bg-red-100 text-red-700 border-red-200';
    default: return 'bg-slate-100 text-slate-700';
  }
};

const getStatusLabel = (status) => {
  switch(status) {
    case 'pending': return 'Pendiente';
    case 'approved': return 'En Preparación'; 
    case 'delivering': return 'En Camino';
    case 'delivered': return 'Entregado';
    case 'cancelled': return 'Anulado';
    case 'cancelled_timeout': return 'Expirado';
    default: return status;
  }
};

// --- COMPONENTS ---

const CategorySelector = ({ selectedCategory, onSelectCategory }) => {
  const [isOpen, setIsOpen] = useState(false);
  const [expandedNodes, setExpandedNodes] = useState({});

  const toggleDropdown = () => setIsOpen(!isOpen);

  const toggleExpand = (e, nodeId) => {
    e.stopPropagation();
    setExpandedNodes(prev => ({
      ...prev,
      [nodeId]: !prev[nodeId]
    }));
  };

  const handleSelect = (value) => {
    onSelectCategory(value);
    setIsOpen(false);
  };

  const renderTree = (nodes, level = 0) => {
    return nodes.map(node => (
      <div key={node.id} className="select-none">
        <div 
          onClick={(e) => node.type === 'parent' ? toggleExpand(e, node.id) : handleSelect(node.value)}
          className={`
            flex items-center justify-between px-4 py-3 cursor-pointer transition-colors
            ${node.value === selectedCategory ? 'bg-orange-50 text-orange-700 font-medium' : 'text-slate-600 hover:bg-slate-50'}
            ${level > 0 ? 'pl-' + (4 + level * 4) : ''}
          `}
        >
          <div className="flex items-center gap-3">
            {node.icon && <node.icon size={18} className={node.value === selectedCategory ? 'text-orange-500' : 'text-slate-400'} />}
            <span className={level > 0 ? 'text-sm' : 'text-sm font-semibold'}>{node.label}</span>
          </div>

          {node.type === 'parent' && (
            <ChevronDown 
              size={16} 
              className={`text-slate-400 transition-transform duration-200 ${expandedNodes[node.id] ? 'rotate-180' : ''}`} 
            />
          )}
          
          {node.type === 'leaf' && node.value === selectedCategory && (
            <Check size={16} className="text-orange-500" />
          )}
        </div>

        {node.type === 'parent' && expandedNodes[node.id] && node.children && (
          <div className="border-l border-slate-100 ml-6">
            {renderTree(node.children, level + 1)}
          </div>
        )}
      </div>
    ));
  };

  return (
    <div className="relative z-40 w-full sm:w-72">
      <div className="flex items-center gap-2 mb-2">
        <div className="flex items-center gap-1 text-slate-400 text-xs font-bold uppercase">
          <Filter size={14} /> Filtrar por Familia:
        </div>
      </div>

      <button 
        onClick={toggleDropdown}
        className="w-full bg-white border border-slate-200 rounded-xl px-4 py-3 flex items-center justify-between shadow-sm hover:border-orange-500 hover:shadow transition-all text-slate-700 font-medium"
      >
        <span className="truncate">{selectedCategory === 'Todos' ? 'Todas las Categorías' : selectedCategory}</span>
        <ChevronDown size={18} className={`transition-transform duration-200 ${isOpen ? 'rotate-180 text-orange-500' : 'text-slate-400'}`} />
      </button>

      {isOpen && (
        <>
          <div 
            className="fixed inset-0 cursor-default" 
            onClick={() => setIsOpen(false)}
          ></div>
          <div className="absolute top-full left-0 mt-2 w-full bg-white border border-slate-200 rounded-xl shadow-xl overflow-hidden animate-in slide-in-from-top-2 duration-200 max-h-96 overflow-y-auto">
            {renderTree(CATEGORY_HIERARCHY)}
          </div>
        </>
      )}
    </div>
  );
};

const InteractiveMap = ({ addressContext, onCoordinatesSelect }) => {
  const [mapStatus, setMapStatus] = useState('idle'); 
  const [pinPosition, setPinPosition] = useState(null); 
  const [searchQuery, setSearchQuery] = useState('');
  const mapRef = useRef(null);

  useEffect(() => {
    if (addressContext.mainStreet && addressContext.city) {
      setMapStatus('locating');
      const timer = setTimeout(() => {
        setMapStatus('ready');
      }, 1500);
      return () => clearTimeout(timer);
    }
  }, [addressContext.mainStreet, addressContext.city, addressContext.neighborhood]);

  const handleMapClick = (e) => {
    if (mapRef.current) {
      const rect = mapRef.current.getBoundingClientRect();
      const x = ((e.clientX - rect.left) / rect.width) * 100;
      const y = ((e.clientY - rect.top) / rect.height) * 100;
      
      setPinPosition({ x, y });
      
      const mockLat = -25.29 + (Math.random() * 0.01);
      const mockLng = -57.60 + (Math.random() * 0.01);
      
      onCoordinatesSelect({ lat: mockLat, lng: mockLng });
    }
  };

  return (
    <div className="relative w-full h-64 bg-slate-100 rounded-xl overflow-hidden border-2 border-slate-200 group">
      <div className="absolute top-3 left-3 right-3 z-10">
        <div className="relative shadow-lg">
          <input 
            type="text" 
            placeholder="Buscar en el mapa..." 
            className="w-full bg-white pl-9 pr-4 py-2 rounded-lg text-xs font-medium focus:outline-none border border-slate-200"
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
          />
          <Search size={14} className="absolute left-3 top-2.5 text-slate-400" />
        </div>
      </div>

      <div 
        ref={mapRef}
        onClick={handleMapClick}
        className={`w-full h-full cursor-crosshair relative transition-all duration-1000 ${mapStatus === 'locating' ? 'scale-110 blur-sm' : 'scale-100 blur-0'}`}
        style={{
          backgroundColor: '#e5e7eb',
          backgroundImage: 'radial-gradient(#cbd5e1 1px, transparent 1px)',
          backgroundSize: '20px 20px'
        }}
      >
        <div className="absolute top-1/2 left-0 w-full h-2 bg-white/50 -translate-y-1/2"></div>
        <div className="absolute top-0 left-1/2 h-full w-2 bg-white/50 -translate-x-1/2"></div>
        
        {pinPosition && (
          <div 
            className="absolute text-orange-600 -translate-x-1/2 -translate-y-full transition-all duration-300 drop-shadow-md"
            style={{ left: `${pinPosition.x}%`, top: `${pinPosition.y}%` }}
          >
            <MapPin size={32} fill="currentColor" />
          </div>
        )}
      </div>

      <div className="absolute bottom-3 right-3 flex flex-col items-end gap-2 pointer-events-none">
        {mapStatus === 'locating' && (
          <span className="bg-slate-800 text-white text-[10px] px-2 py-1 rounded-md animate-pulse">
            Acercando ubicación...
          </span>
        )}
        {pinPosition && (
          <span className="bg-green-600 text-white text-[10px] px-2 py-1 rounded-md font-bold shadow-sm flex items-center gap-1">
            <Check size={10} /> Coordenadas fijadas
          </span>
        )}
      </div>

      {!pinPosition && mapStatus !== 'locating' && (
        <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
          <div className="bg-white/80 backdrop-blur-sm px-4 py-2 rounded-full text-xs text-slate-500 font-medium border border-slate-200">
            Haz click para marcar la entrega
          </div>
        </div>
      )}
    </div>
  );
};

const ProductDetailView = ({ product, onBack, onAddToCart }) => {
  if (!product) return null;

  const totalStock = product.stock.central + product.stock.capuchi + product.stock.family;

  const handleShare = async () => {
    if (navigator.share) {
      try {
        await navigator.share({
          title: product.name,
          text: `Mira este producto en Family Center: ${product.name} a Gs. ${product.price.toLocaleString()}`,
          url: window.location.href, 
        });
      } catch (error) {
        console.log('Error sharing', error);
      }
    } else {
      alert("Enlace copiado al portapapeles");
    }
  };

  const handleWhatsApp = () => {
    const text = encodeURIComponent(`Hola, quisiera consultar disponibilidad sobre: ${product.name} (Ref: ${product.id})`);
    window.open(`https://wa.me/?text=${text}`, '_blank');
  };
  
  return (
    <div className="flex flex-col h-full animate-in slide-in-from-right duration-300">
      <div className="flex items-center gap-2 mb-6">
        <button 
          onClick={onBack}
          className="p-2 hover:bg-slate-200 rounded-full transition-colors group"
        >
          <ArrowLeft size={24} className="text-slate-600 group-hover:text-slate-900" />
        </button>
        <h2 className="text-xl font-bold text-slate-800">Detalles del Producto</h2>
      </div>

      <div className="flex flex-col lg:flex-row gap-8 bg-white rounded-2xl shadow-sm border border-slate-100 p-6 flex-1">
        <div className="w-full lg:w-1/2 flex flex-col">
          <div className="flex-1 bg-slate-50 rounded-xl flex items-center justify-center p-8 min-h-[300px] relative overflow-hidden group">
            <div className="absolute inset-0 bg-slate-100 opacity-0 group-hover:opacity-10 transition-opacity"></div>
            <Smartphone size={120} className="text-slate-300 drop-shadow-2xl transition-transform duration-500 group-hover:scale-110" />
            <span className="absolute top-4 left-4 bg-slate-900 text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider shadow-lg">
              {product.category}
            </span>
          </div>
        </div>

        <div className="w-full lg:w-1/2 flex flex-col">
          <div className="flex-1">
            <h1 className="text-3xl font-black text-slate-900 leading-tight mb-2">{product.name}</h1>
            <p className="text-2xl font-bold text-orange-600 mb-6">Gs. {product.price.toLocaleString()}</p>

            <div className="prose text-slate-600 text-sm mb-6 leading-relaxed">
              <p>{product.description}</p>
            </div>

            <div className="flex flex-wrap gap-2 mb-8">
              {product.features.map((feat, idx) => (
                <span key={idx} className="bg-slate-100 text-slate-600 text-xs font-medium px-3 py-1.5 rounded-lg border border-slate-200">
                  {feat}
                </span>
              ))}
            </div>

            <div className="bg-slate-50 rounded-xl p-5 border border-slate-200 mb-8">
              <h3 className="text-xs font-bold text-slate-500 uppercase mb-4 flex items-center gap-2">
                <Store size={16} /> Disponibilidad en Tiempo Real
              </h3>
              <div className="grid grid-cols-3 gap-4">
                <div className={`flex flex-col items-center p-3 rounded-lg border bg-white ${product.stock.central > 0 ? 'border-green-200 shadow-sm' : 'border-slate-100 opacity-50'}`}>
                  <span className="text-[10px] text-slate-400 font-bold uppercase mb-1">Central</span>
                  <span className={`text-2xl font-bold ${product.stock.central > 0 ? 'text-green-600' : 'text-slate-300'}`}>
                    {product.stock.central}
                  </span>
                </div>
                <div className={`flex flex-col items-center p-3 rounded-lg border bg-white ${product.stock.capuchi > 0 ? 'border-green-200 shadow-sm' : 'border-slate-100 opacity-50'}`}>
                  <span className="text-[10px] text-slate-400 font-bold uppercase mb-1">Capuchi</span>
                  <span className={`text-2xl font-bold ${product.stock.capuchi > 0 ? 'text-green-600' : 'text-slate-300'}`}>
                    {product.stock.capuchi}
                  </span>
                </div>
                <div className={`flex flex-col items-center p-3 rounded-lg border bg-white ${product.stock.family > 0 ? 'border-green-200 shadow-sm' : 'border-slate-100 opacity-50'}`}>
                  <span className="text-[10px] text-slate-400 font-bold uppercase mb-1">Family</span>
                  <span className={`text-2xl font-bold ${product.stock.family > 0 ? 'text-green-600' : 'text-slate-300'}`}>
                    {product.stock.family}
                  </span>
                </div>
              </div>
              {totalStock === 0 && (
                <div className="mt-4 text-center text-sm text-red-600 font-bold bg-red-50 p-2 rounded-lg border border-red-100 flex items-center justify-center gap-2">
                  <AlertTriangle size={16} /> Sin stock disponible en la red
                </div>
              )}
            </div>
          </div>

          <div className="grid grid-cols-2 sm:grid-cols-4 gap-3 mt-auto pt-6 border-t border-slate-100">
            <button 
              onClick={handleShare}
              className="col-span-1 flex flex-col items-center justify-center gap-1 bg-white border border-slate-200 text-slate-600 py-3 rounded-xl hover:bg-slate-50 hover:text-slate-900 transition-colors"
            >
              <Share2 size={20} />
              <span className="text-[10px] font-bold uppercase">Compartir</span>
            </button>
            
            <button 
              onClick={handleWhatsApp}
              className="col-span-1 flex flex-col items-center justify-center gap-1 bg-white border border-green-200 text-green-600 py-3 rounded-xl hover:bg-green-50 transition-colors"
            >
              <MessageCircle size={20} />
              <span className="text-[10px] font-bold uppercase">Consultar</span>
            </button>

            <button 
              onClick={() => onAddToCart(product)}
              disabled={totalStock === 0}
              className={`col-span-2 py-3 rounded-xl font-bold flex items-center justify-center gap-2 shadow-lg transition-all text-sm sm:text-base ${
                totalStock > 0 
                  ? 'bg-slate-900 text-white hover:bg-orange-600 shadow-orange-500/20' 
                  : 'bg-slate-200 text-slate-400 cursor-not-allowed shadow-none'
              }`}
            >
              {totalStock > 0 ? (
                <>
                  <ShoppingCart size={20} /> AGREGAR AL PEDIDO
                </>
              ) : (
                <>
                  <XCircle size={20} /> AGOTADO
                </>
              )}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

const CartCheckoutView = ({ cart, onRemoveFromCart, onAddOne, onRemoveOne, onBackToCatalog, onConfirmOrder }) => {
  const [step, setStep] = useState(1);
  
  // Checkout State
  const [deliveryType, setDeliveryType] = useState('delivery'); 
  const [selectedZone, setSelectedZone] = useState(ZONES[0]);
  const [selectedStore, setSelectedStore] = useState(null);
  
  // Address State
  const [addressData, setAddressData] = useState({
    mainStreet: '',
    crossStreet: '',
    neighborhood: '',
    city: ZONES[0].cities[0], 
    coordinates: null
  });

  const [formData, setFormData] = useState({
    name: '',
    ruc: '',
    phone: '' 
  });

  const groupedCart = cart.reduce((acc, item) => {
    const existing = acc.find(i => i.id === item.id);
    if (existing) {
      existing.quantity += 1;
    } else {
      acc.push({ ...item, quantity: 1 });
    }
    return acc;
  }, []);

  const subtotal = cart.reduce((sum, item) => sum + item.price, 0);
  const deliveryCost = deliveryType === 'pickup' ? 0 : selectedZone.price;
  const total = subtotal + deliveryCost;

  const availableCities = selectedZone.cities || [selectedZone.name];

  const handleNext = () => {
    // ... existing logic ...
    if (step === 2) {
      if (deliveryType === 'delivery') {
        if (!addressData.mainStreet || !addressData.neighborhood || !addressData.city) {
          alert("Por favor completa los campos de Calle Principal, Barrio y Ciudad.");
          return;
        }
        if (!addressData.coordinates) {
          alert("Por favor marca la ubicación exacta en el mapa.");
          return;
        }
      }
      if (deliveryType === 'pickup' && !selectedStore) {
        alert("Por favor selecciona un local para el retiro.");
        return;
      }
      setStep(step + 1);
    } else if (step === 3) {
      if (!formData.name.trim() || !formData.phone.trim()) {
        alert("Nombre y Celular son obligatorios.");
        return;
      }
      setStep(step + 1);
    } else if (step === 4) {
      onConfirmOrder({
        ...formData,
        deliveryType,
        zone: deliveryType === 'pickup' ? { name: `Retiro: ${selectedStore.name}`, price: 0 } : selectedZone,
        address: deliveryType === 'delivery' ? addressData : null,
        total,
      });
    } else {
      setStep(step + 1);
    }
  };

  const handleBack = () => {
    if (step === 1) {
      onBackToCatalog();
    } else {
      setStep(step - 1);
    }
  };

  const getStoreAvailability = (storeId) => {
    // ... existing logic ...
    let availableCount = 0;
    let missingItems = [];
    const cartCounts = {};
    cart.forEach(item => {
      cartCounts[item.id] = (cartCounts[item.id] || 0) + 1;
    });

    Object.keys(cartCounts).forEach(prodId => {
      const product = cart.find(p => p.id === parseInt(prodId));
      const required = cartCounts[prodId];
      const available = product.stock[storeId] || 0;
      
      if (available >= required) {
        availableCount += required;
      } else {
        availableCount += available;
        missingItems.push(product.name);
      }
    });

    return {
      isFullyAvailable: missingItems.length === 0,
      missingItems,
      availableCount,
      totalItems: cart.length
    };
  };

  const handleRemoveAll = (itemId) => {
    const indexesToRemove = cart
        .map((item, idx) => item.id === itemId ? idx : -1)
        .filter(idx => idx !== -1)
        .sort((a, b) => b - a);
    
    indexesToRemove.forEach(index => onRemoveFromCart(index));
  };

  const handleRemoveSingle = (itemId) => {
    const indexToRemove = cart.findIndex(item => item.id === itemId);
    if (indexToRemove !== -1) {
      onRemoveFromCart(indexToRemove);
    }
  }

  return (
    <div className="flex flex-col h-full animate-in slide-in-from-right duration-300 max-w-4xl mx-auto">
      {/* Navbar & Stepper */}
      <div className="mb-6">
        <div className="flex items-center gap-2 mb-4">
          <button onClick={handleBack} className="p-2 hover:bg-slate-200 rounded-full transition-colors text-slate-600">
            <ArrowLeft size={24} />
          </button>
          <h2 className="text-xl font-bold text-slate-800">
            {step === 1 && "1. Revisar Productos"}
            {step === 2 && "2. Logística y Ubicación"}
            {step === 3 && "3. Datos de Facturación"}
            {step === 4 && "4. Confirmar y Generar"}
          </h2>
        </div>
        
        <div className="flex items-center gap-2 w-full px-2">
          <div className={`h-2 flex-1 rounded-full transition-colors ${step >= 1 ? 'bg-orange-500' : 'bg-slate-200'}`}></div>
          <div className={`h-2 flex-1 rounded-full transition-colors ${step >= 2 ? 'bg-orange-500' : 'bg-slate-200'}`}></div>
          <div className={`h-2 flex-1 rounded-full transition-colors ${step >= 3 ? 'bg-orange-500' : 'bg-slate-200'}`}></div>
          <div className={`h-2 flex-1 rounded-full transition-colors ${step >= 4 ? 'bg-orange-500' : 'bg-slate-200'}`}></div>
        </div>
      </div>

      <div className="flex-1 bg-white rounded-3xl shadow-sm border border-slate-200 overflow-hidden flex flex-col">
        <div className="flex-1 overflow-y-auto p-6 bg-slate-50/50">
          
          {/* STEP 1: REVIEW (REDESIGNED) */}
          {step === 1 && (
            <div className="space-y-4">
              {groupedCart.length === 0 ? (
                <div className="text-center py-20 text-slate-400">
                  <ShoppingCart size={48} className="mx-auto mb-4 opacity-30" />
                  <p>Tu carrito está vacío.</p>
                </div>
              ) : (
                <>
                  {/* Table Header (Hidden on Mobile) */}
                  <div className="hidden sm:grid sm:grid-cols-[60px_2fr_120px_100px_100px_40px] gap-4 px-4 pb-2 border-b border-slate-200 text-xs font-bold text-slate-400 uppercase tracking-wider">
                    <div></div> {/* Image placeholder */}
                    <div>Producto</div>
                    <div className="text-center">Cantidad</div>
                    <div className="text-right">Unitario</div>
                    <div className="text-right">Total</div>
                    <div></div> {/* Actions */}
                  </div>

                  {/* List Items */}
                  {groupedCart.map((item) => {
                    const totalStock = item.stock.central + item.stock.capuchi + item.stock.family;
                    return (
                      <div 
                        key={item.id} 
                        className="group bg-white p-4 rounded-xl shadow-sm border border-slate-200 hover:border-orange-300 transition-all 
                                   grid grid-cols-1 sm:grid-cols-[60px_2fr_120px_100px_100px_40px] gap-4 items-center"
                      >
                        
                        {/* 1. Image */}
                        <div className="w-14 h-14 bg-slate-50 rounded-lg flex items-center justify-center border border-slate-100 mx-auto sm:mx-0">
                           <Smartphone size={24} className="text-slate-400" />
                        </div>

                        {/* 2. Description */}
                        <div className="text-center sm:text-left">
                          <h4 className="font-bold text-slate-800 text-sm leading-tight truncate">{item.name}</h4>
                          <span className="text-[10px] text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full font-medium inline-block mt-1">
                            {item.category}
                          </span>
                        </div>

                        {/* 3. Quantity Controls */}
                        <div className="flex items-center justify-center gap-2 bg-slate-50 rounded-lg p-1 border border-slate-100 w-fit mx-auto sm:w-full">
                          <button 
                            onClick={() => handleRemoveSingle(item.id)}
                            className="w-7 h-7 flex items-center justify-center bg-white rounded-md shadow-sm text-slate-600 hover:text-orange-600 transition-colors"
                          >
                            <Minus size={14} />
                          </button>
                          <span className="text-sm font-bold text-slate-800 w-8 text-center">{item.quantity}</span>
                          <button 
                            onClick={() => onAddOne(item)}
                            disabled={totalStock <= 0}
                            className="w-7 h-7 flex items-center justify-center bg-white rounded-md shadow-sm text-slate-600 hover:text-green-600 transition-colors disabled:opacity-50"
                          >
                            <Plus size={14} />
                          </button>
                        </div>

                        {/* 4. Unit Price */}
                        <div className="flex justify-between sm:block w-full sm:w-auto text-right">
                            <span className="sm:hidden text-xs text-slate-400 font-medium">Unitario:</span>
                            <p className="text-xs font-medium text-slate-500">Gs. {item.price.toLocaleString()}</p>
                        </div>

                        {/* 5. Total Price */}
                        <div className="flex justify-between sm:block w-full sm:w-auto text-right">
                            <span className="sm:hidden text-xs text-slate-400 font-bold">Subtotal:</span>
                            <p className="text-sm font-black text-slate-800">Gs. {(item.price * item.quantity).toLocaleString()}</p>
                        </div>

                        {/* 6. Delete Action */}
                        <div className="text-center sm:text-right">
                          <button 
                            onClick={() => handleRemoveAll(item.id)} 
                            className="p-2 rounded-full text-slate-300 hover:bg-red-50 hover:text-red-500 transition-colors"
                            title="Eliminar ítem"
                          >
                            <Trash2 size={18} />
                          </button>
                        </div>

                      </div>
                    );
                  })}
                </>
              )}
              
              <button 
                onClick={onBackToCatalog}
                className="w-full py-4 border-2 border-dashed border-slate-300 rounded-2xl text-slate-500 font-bold flex items-center justify-center gap-2 hover:bg-white hover:border-orange-300 hover:text-orange-600 transition-all mt-6"
              >
                <Plus size={18} /> Agregar más productos
              </button>
            </div>
          )}

          {/* STEP 2: DELIVERY & ADDRESS */}
          {step === 2 && (
            <div className="space-y-6">
              <div className="grid grid-cols-2 gap-4">
                <button
                  onClick={() => setDeliveryType('pickup')}
                  className={`p-4 rounded-xl border-2 flex flex-col items-center gap-3 transition-all ${
                    deliveryType === 'pickup' 
                      ? 'border-orange-500 bg-orange-50 text-orange-700' 
                      : 'border-slate-200 text-slate-500 hover:bg-slate-50'
                  }`}
                >
                  <Store size={32} />
                  <span className="font-bold text-sm">Retiro en Local</span>
                </button>
                <button
                  onClick={() => setDeliveryType('delivery')}
                  className={`p-4 rounded-xl border-2 flex flex-col items-center gap-3 transition-all ${
                    deliveryType === 'delivery' 
                      ? 'border-orange-500 bg-orange-50 text-orange-700' 
                      : 'border-slate-200 text-slate-500 hover:bg-slate-50'
                  }`}
                >
                  <Truck size={32} />
                  <span className="font-bold text-sm">Delivery</span>
                </button>
              </div>

              {/* Delivery Form */}
              {deliveryType === 'delivery' && (
                <div className="space-y-4 animate-in slide-in-from-top-2 duration-300">
                  
                  {/* Zone Selector */}
                  <div>
                    <label className="block text-sm font-bold text-slate-700 mb-2">Zona de Cobertura</label>
                    <select 
                      className="w-full border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none bg-white"
                      value={selectedZone.id}
                      onChange={(e) => {
                        const zone = ZONES.find(z => z.id === parseInt(e.target.value));
                        setSelectedZone(zone);
                        setAddressData(prev => ({ ...prev, city: zone.cities[0] }));
                      }}
                    >
                      {ZONES.map(z => (
                        <option key={z.id} value={z.id}>{z.name} - Gs. {z.price.toLocaleString()}</option>
                      ))}
                    </select>
                  </div>

                  {/* Detailed Address Fields */}
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                      <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Calle Principal</label>
                      <input 
                        type="text" 
                        placeholder="Ej. Av. Mariscal López"
                        className="w-full border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                        value={addressData.mainStreet}
                        onChange={(e) => setAddressData({...addressData, mainStreet: e.target.value})}
                      />
                    </div>
                    <div>
                      <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Calle Lateral (Ref)</label>
                      <input 
                        type="text" 
                        placeholder="Casi San Martín"
                        className="w-full border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                        value={addressData.crossStreet}
                        onChange={(e) => setAddressData({...addressData, crossStreet: e.target.value})}
                      />
                    </div>
                    <div>
                      <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Barrio</label>
                      <input 
                        type="text" 
                        placeholder="Ej. Villa Morra"
                        className="w-full border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                        value={addressData.neighborhood}
                        onChange={(e) => setAddressData({...addressData, neighborhood: e.target.value})}
                      />
                    </div>
                    <div>
                      <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Ciudad</label>
                      <select 
                        className="w-full border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none bg-white"
                        value={addressData.city}
                        onChange={(e) => setAddressData({...addressData, city: e.target.value})}
                      >
                        {availableCities.map(city => (
                          <option key={city} value={city}>{city}</option>
                        ))}
                      </select>
                    </div>
                  </div>

                  {/* Interactive Map */}
                  <div className="mt-2">
                    <label className="block text-sm font-bold text-slate-700 mb-2 flex items-center justify-between">
                      <span>Ubicación Exacta</span>
                      <span className="text-xs font-normal text-slate-500 bg-slate-100 px-2 py-0.5 rounded">Click en mapa para fijar</span>
                    </label>
                    <InteractiveMap 
                      addressContext={addressData} 
                      onCoordinatesSelect={(coords) => setAddressData({...addressData, coordinates: coords})}
                    />
                  </div>

                </div>
              )}

              {/* Pickup Option Logic */}
              {deliveryType === 'pickup' && (
                <div className="space-y-3 animate-in slide-in-from-top-2">
                  <p className="text-sm font-bold text-slate-700 mb-1">Selecciona la sucursal de retiro:</p>
                  
                  {STORES.map(store => {
                    const availability = getStoreAvailability(store.id);
                    const isSelected = selectedStore?.id === store.id;
                    
                    return (
                      <div 
                        key={store.id}
                        onClick={() => setSelectedStore(store)}
                        className={`
                          relative p-4 rounded-xl border-2 transition-all cursor-pointer flex flex-col gap-2
                          ${isSelected 
                            ? 'border-orange-500 bg-white shadow-md' 
                            : 'border-slate-100 bg-slate-50 hover:border-slate-300'
                          }
                        `}
                      >
                        <div className="flex justify-between items-start">
                          <div>
                            <h4 className="font-bold text-slate-800">{store.name}</h4>
                            <p className="text-xs text-slate-500">{store.address}</p>
                          </div>
                          {isSelected && <CheckCircle className="text-orange-500" size={20} />}
                        </div>

                        <div className="mt-1 pt-2 border-t border-dashed border-slate-200">
                          {availability.isFullyAvailable ? (
                            <span className="flex items-center gap-1.5 text-xs font-bold text-green-600 bg-green-50 px-2 py-1 rounded w-fit">
                              <Check size={12} /> Stock Disponible ({availability.availableCount}/{availability.totalItems})
                            </span>
                          ) : (
                            <div className="flex flex-col gap-1">
                              <span className="flex items-center gap-1.5 text-xs font-bold text-red-500 bg-red-50 px-2 py-1 rounded w-fit">
                                <AlertTriangle size={12} /> Stock Incompleto
                              </span>
                              <p className="text-[10px] text-slate-500 pl-1">
                                Falta: {availability.missingItems.join(', ')}
                              </p>
                            </div>
                          )}
                        </div>
                      </div>
                    );
                  })}
                </div>
              )}
            </div>
          )}

          {/* STEP 3: BILLING */}
          {step === 3 && (
            <div className="space-y-4">
              <div>
                <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Nombre y Apellido <span className="text-red-500">*</span></label>
                <div className="relative">
                  <User className="absolute left-3 top-3 text-slate-400" size={18} />
                  <input 
                    type="text" 
                    className="w-full pl-10 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                    placeholder="Ej. Juan Pérez"
                    value={formData.name}
                    onChange={(e) => setFormData({...formData, name: e.target.value})}
                  />
                </div>
              </div>

              <div>
                <label className="block text-xs font-bold text-slate-500 uppercase mb-1">Celular <span className="text-red-500">*</span></label>
                <div className="relative">
                  <Phone className="absolute left-3 top-3 text-slate-400" size={18} />
                  <input 
                    type="tel" 
                    className="w-full pl-10 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                    placeholder="09xx xxx xxx"
                    value={formData.phone}
                    onChange={(e) => setFormData({...formData, phone: e.target.value})}
                  />
                </div>
              </div>

              <div>
                <label className="block text-xs font-bold text-slate-500 uppercase mb-1">RUC / CI <span className="text-slate-400 font-normal">(Opcional)</span></label>
                <div className="relative">
                  <FileText className="absolute left-3 top-3 text-slate-400" size={18} />
                  <input 
                    type="text" 
                    className="w-full pl-10 border border-slate-300 rounded-xl p-3 text-sm focus:ring-2 focus:ring-orange-500 outline-none"
                    placeholder="Sin RUC"
                    value={formData.ruc}
                    onChange={(e) => setFormData({...formData, ruc: e.target.value})}
                  />
                </div>
              </div>
            </div>
          )}

          {/* STEP 4: SUMMARY & CONFIRMATION (NEW) */}
          {step === 4 && (
            <div className="space-y-6 animate-in slide-in-from-right duration-300">
              
              {/* Product Summary */}
              <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                <h3 className="flex items-center gap-2 font-bold text-slate-700 mb-4 pb-2 border-b border-slate-100">
                  <Package size={18} className="text-orange-500" /> Resumen de Productos
                </h3>
                <div className="space-y-3">
                  {groupedCart.map((item, idx) => (
                    <div key={idx} className="flex justify-between items-center text-sm">
                      <div className="flex gap-2 items-center text-slate-600 font-medium truncate pr-4">
                        <span className="bg-slate-100 text-slate-500 px-2 py-0.5 rounded text-xs">x{item.quantity}</span>
                        <span>{item.name}</span>
                      </div>
                      <span className="font-bold text-slate-900 whitespace-nowrap">Gs. {(item.price * item.quantity).toLocaleString()}</span>
                    </div>
                  ))}
                </div>
              </div>

              {/* Delivery Info Summary */}
              <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                <h3 className="flex items-center gap-2 font-bold text-slate-700 mb-4 pb-2 border-b border-slate-100">
                  <Truck size={18} className="text-orange-500" /> Datos de Entrega
                </h3>
                
                {deliveryType === 'delivery' ? (
                  <div className="space-y-2 text-sm">
                    <div className="flex items-start gap-2">
                      <MapPin size={16} className="text-slate-400 mt-0.5" />
                      <div>
                        <p className="font-bold text-slate-800">{addressData.mainStreet}</p>
                        <p className="text-slate-500">Ref: {addressData.crossStreet}</p>
                        <p className="text-slate-500">{addressData.neighborhood}, {addressData.city}</p>
                      </div>
                    </div>
                    {addressData.coordinates && (
                      <div className="flex items-center gap-2 text-green-600 text-xs font-bold bg-green-50 px-2 py-1 rounded w-fit mt-1">
                        <Crosshair size={12} /> Ubicación marcada en mapa
                      </div>
                    )}
                  </div>
                ) : (
                  <div className="flex items-start gap-2 text-sm">
                    <Store size={16} className="text-slate-400 mt-0.5" />
                    <div>
                      <p className="font-bold text-slate-800">Retiro en: {selectedStore?.name}</p>
                      <p className="text-slate-500">{selectedStore?.address}</p>
                    </div>
                  </div>
                )}
              </div>

              {/* Billing Info Summary */}
              <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                <h3 className="flex items-center gap-2 font-bold text-slate-700 mb-4 pb-2 border-b border-slate-100">
                  <Receipt size={18} className="text-orange-500" /> Datos de Facturación
                </h3>
                <div className="grid grid-cols-2 gap-4 text-sm">
                  <div>
                    <p className="text-xs text-slate-400 uppercase font-bold">Cliente</p>
                    <p className="font-medium text-slate-800">{formData.name}</p>
                  </div>
                  <div>
                    <p className="text-xs text-slate-400 uppercase font-bold">Celular</p>
                    <p className="font-medium text-slate-800">{formData.phone}</p>
                  </div>
                  <div className="col-span-2">
                    <p className="text-xs text-slate-400 uppercase font-bold">RUC / CI</p>
                    <p className="font-medium text-slate-800">{formData.ruc || 'Sin RUC'}</p>
                  </div>
                </div>
              </div>

            </div>
          )}
        </div>

        {/* Footer Actions */}
        <div className="p-6 border-t border-slate-100 bg-white">
          <div className="flex justify-between items-end mb-4">
            <div className="text-sm text-slate-500">
              <p>Subtotal: Gs. {subtotal.toLocaleString()}</p>
              <p>Delivery: Gs. {deliveryCost.toLocaleString()}</p>
            </div>
            <div className="text-right">
              <p className="text-xs text-slate-400 uppercase font-bold">Total Final</p>
              <p className="text-2xl font-black text-slate-900 leading-none">Gs. {total.toLocaleString()}</p>
            </div>
          </div>

          <div className="flex gap-3">
            {step > 1 && (
              <button 
                onClick={handleBack}
                className="w-1/3 bg-white border-2 border-slate-100 text-slate-500 py-4 rounded-xl font-bold hover:bg-slate-50 hover:text-slate-800 transition-all"
              >
                Atrás
              </button>
            )}

            <button 
              onClick={handleNext}
              disabled={cart.length === 0}
              className={`flex-1 bg-slate-900 text-white py-4 rounded-xl font-bold hover:bg-orange-600 transition-all shadow-lg active:scale-95 flex items-center justify-center gap-2 disabled:opacity-50 disabled:cursor-not-allowed`}
            >
              {step === 4 ? (
                <><CheckCircle size={20} /> CONFIRMAR</>
              ) : (
                <><span className="mr-1">Siguiente</span> <ChevronRight size={20} /></>
              )}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

// 5. Order Detail View (Unchanged)
const OrderDetailView = ({ order, onBack }) => {
  if (!order) return null;

  const timelineSteps = [
    { status: 'pending', label: 'Pendiente' },
    { status: 'approved', label: 'En Preparación' },
    { status: 'delivering', label: 'En Camino' },
    { status: 'delivered', label: 'Entregado' }
  ];

  const currentStepIndex = timelineSteps.findIndex(s => s.status === order.status);
  const isCancelled = order.status.includes('cancelled');

  return (
    <div className="flex flex-col h-full animate-in slide-in-from-right duration-300 max-w-4xl mx-auto">
      {/* Navbar */}
      <div className="flex items-center justify-between mb-6">
        <div className="flex items-center gap-2">
          <button 
            onClick={onBack}
            className="p-2 hover:bg-slate-200 rounded-full transition-colors group"
          >
            <ArrowLeft size={24} className="text-slate-600 group-hover:text-slate-900" />
          </button>
          <div>
            <h2 className="text-xl font-bold text-slate-800">Pedido #{order.id}</h2>
            <p className="text-xs text-slate-500">{order.timestamp.toLocaleString()}</p>
          </div>
        </div>
        <span className={`px-3 py-1 rounded-full text-xs font-bold border ${getStatusColor(order.status)}`}>
          {getStatusLabel(order.status)}
        </span>
      </div>

      {/* Content */}
      <div className="space-y-6">
        
        {/* Status Timeline */}
        <div className="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm">
          {isCancelled ? (
            <div className="flex items-center justify-center gap-2 text-red-500 font-bold p-4 bg-red-50 rounded-xl">
              <XCircle /> Pedido Cancelado / Expirado
            </div>
          ) : (
            <div className="flex justify-between relative">
              {/* Connector Line */}
              <div className="absolute top-1/2 left-0 w-full h-1 bg-slate-100 -z-10 -translate-y-1/2 rounded-full"></div>
              <div 
                className="absolute top-1/2 left-0 h-1 bg-green-500 -z-10 -translate-y-1/2 rounded-full transition-all duration-500"
                style={{ width: `${(currentStepIndex / (timelineSteps.length - 1)) * 100}%` }}
              ></div>

              {timelineSteps.map((step, idx) => {
                const isActive = idx <= currentStepIndex;
                return (
                  <div key={step.status} className="flex flex-col items-center gap-2 bg-white px-2">
                    <div className={`w-8 h-8 rounded-full flex items-center justify-center border-2 transition-colors ${isActive ? 'bg-green-500 border-green-500 text-white' : 'bg-white border-slate-300 text-slate-300'}`}>
                      {isActive ? <Check size={14} /> : <div className="w-2 h-2 bg-slate-300 rounded-full"></div>}
                    </div>
                    <span className={`text-[10px] font-bold uppercase ${isActive ? 'text-slate-800' : 'text-slate-400'}`}>
                      {step.label}
                    </span>
                  </div>
                );
              })}
            </div>
          )}
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          {/* Client & Delivery Info */}
          <div className="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col gap-4">
            <h3 className="text-sm font-bold text-slate-500 uppercase flex items-center gap-2 border-b border-slate-100 pb-2">
              <User size={16} /> Información del Cliente
            </h3>
            
            <div className="flex items-center gap-4">
              <div className="w-10 h-10 bg-slate-100 rounded-full flex items-center justify-center text-slate-400">
                <User size={20} />
              </div>
              <div>
                <p className="font-bold text-slate-800">{order.client}</p>
                <p className="text-sm text-slate-500 flex items-center gap-1">
                  <FileText size={12} /> {order.ruc}
                </p>
                {/* Phone number display would go here if added to order object */}
              </div>
            </div>

            <div className="mt-2">
              <p className="text-xs font-bold text-slate-400 uppercase mb-1">Dirección de Entrega</p>
              <div className="flex items-start gap-2 bg-slate-50 p-3 rounded-lg border border-slate-100">
                <MapPin size={16} className="text-orange-500 mt-0.5" />
                <span className="text-sm text-slate-700 font-medium">{order.zone.name}</span>
              </div>
            </div>
          </div>

          {/* Items & Totals */}
          <div className="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col">
            <h3 className="text-sm font-bold text-slate-500 uppercase flex items-center gap-2 border-b border-slate-100 pb-2 mb-4">
              <Package size={16} /> Productos
            </h3>
            
            <div className="flex-1 space-y-3 mb-6">
              {order.items.map((item, idx) => (
                <div key={idx} className="flex justify-between items-center text-sm">
                  <div className="flex items-center gap-2">
                    <div className="w-8 h-8 bg-slate-50 rounded flex items-center justify-center text-slate-400 border border-slate-100">
                      <Smartphone size={14} />
                    </div>
                    <span className="text-slate-700 font-medium">{item.name}</span>
                  </div>
                  <span className="text-slate-900 font-bold">Gs. {item.price.toLocaleString()}</span>
                </div>
              ))}
            </div>

            <div className="border-t border-slate-100 pt-4 space-y-2">
              <div className="flex justify-between text-sm text-slate-500">
                <span>Subtotal Productos</span>
                <span>Gs. {order.items.reduce((sum, i) => sum + i.price, 0).toLocaleString()}</span>
              </div>
              <div className="flex justify-between text-sm text-slate-500">
                <span>Costo Delivery</span>
                <span>Gs. {order.zone.price.toLocaleString()}</span>
              </div>
              <div className="flex justify-between text-xl font-bold text-slate-900 pt-2 border-t border-dashed border-slate-200">
                <span>Total General</span>
                <span>Gs. {order.total.toLocaleString()}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

// 6. Login Component (Unchanged)
const Login = ({ onLogin }) => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false);

  const handleTraditionalLogin = (e) => {
    e.preventDefault();
    setError('');
    setLoading(true);

    setTimeout(() => {
      setLoading(false);
      if (!username || !password) {
        setError('Por favor complete todos los campos.');
        return;
      }
      if (username.toLowerCase() === 'admin' && password === 'admin') {
        onLogin('prep', 'Administrador');
      } else if (username.toLowerCase() === 'moto' && password === '123') {
        onLogin('delivery', 'Logística');
      } else {
        onLogin('seller', username);
      }
    }, 800);
  };

  return (
    <div className="min-h-screen bg-slate-100 flex items-center justify-center p-4">
      <div className="bg-white rounded-2xl shadow-2xl w-full max-w-4xl flex flex-col md:flex-row overflow-hidden">
        <div className="bg-slate-900 p-8 md:w-1/2 text-white flex flex-col justify-between relative overflow-hidden">
          <div className="absolute top-0 right-0 -mr-16 -mt-16 w-64 h-64 bg-orange-500 rounded-full opacity-20 blur-3xl"></div>
          <div className="absolute bottom-0 left-0 -ml-16 -mb-16 w-64 h-64 bg-blue-500 rounded-full opacity-20 blur-3xl"></div>
          <div className="relative z-10">
            <div className="flex items-center gap-2 mb-6">
              <div className="bg-orange-500 p-2 rounded text-white font-bold text-xl">FC</div>
              <span className="font-bold text-2xl tracking-tight">FAMILY CENTER</span>
            </div>
            <h2 className="text-3xl font-bold mb-4 leading-tight">Gestión Integral de Ventas</h2>
            <p className="text-slate-400 mb-8">
              Plataforma optimizada para vendedores externos. Consulta stock en tiempo real, gestiona pedidos y coordina entregas.
            </p>
          </div>
          <div className="space-y-4 relative z-10 text-sm text-slate-400">
            <div className="flex items-center gap-3">
              <CheckCircle className="text-orange-500" size={20} />
              <span>Stock sincronizado (Central/Sucursales)</span>
            </div>
            <div className="flex items-center gap-3">
              <CheckCircle className="text-orange-500" size={20} />
              <span>Cálculo automático de delivery</span>
            </div>
          </div>
        </div>
        <div className="p-8 md:w-1/2 bg-white flex flex-col justify-center">
          <h3 className="text-2xl font-bold text-slate-800 mb-6">Iniciar Sesión</h3>
          <form onSubmit={handleTraditionalLogin} className="space-y-5">
            <div>
              <label className="block text-sm font-bold text-slate-600 mb-1">Usuario</label>
              <div className="relative">
                <User className="absolute left-3 top-3 text-slate-400" size={20} />
                <input 
                  type="text" 
                  value={username}
                  onChange={(e) => setUsername(e.target.value)}
                  className="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 text-slate-800"
                  placeholder="Ej. vendedor1"
                />
              </div>
            </div>
            <div>
              <label className="block text-sm font-bold text-slate-600 mb-1">Contraseña</label>
              <div className="relative">
                <Lock className="absolute left-3 top-3 text-slate-400" size={20} />
                <input 
                  type="password" 
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  className="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 text-slate-800"
                  placeholder="••••••••"
                />
              </div>
            </div>
            {error && (
              <div className="p-3 bg-red-50 text-red-600 text-sm rounded-lg flex items-center gap-2">
                <AlertTriangle size={16} />
                {error}
              </div>
            )}
            <button 
              type="submit" 
              disabled={loading}
              className="w-full bg-slate-900 text-white font-bold py-3 rounded-lg hover:bg-slate-800 transition-all flex items-center justify-center gap-2 shadow-lg disabled:opacity-70"
            >
              {loading ? <div className="w-5 h-5 border-2 border-white/30 border-t-white rounded-full animate-spin"></div> : <>INGRESAR AL SISTEMA <ChevronRight size={18} /></>}
            </button>
          </form>
          <div className="mt-8 pt-6 border-t border-slate-100">
            <p className="text-xs font-bold text-slate-400 uppercase text-center mb-4">Accesos Directos (Modo Demo)</p>
            <div className="grid grid-cols-3 gap-2">
              <button onClick={() => onLogin('seller', 'Demo Vendedor')} className="p-2 border border-slate-200 rounded hover:bg-orange-50 text-xs text-slate-600 flex flex-col items-center gap-1">
                <User size={16} className="text-orange-500"/> Vendedor
              </button>
              <button onClick={() => onLogin('prep', 'Demo Admin')} className="p-2 border border-slate-200 rounded hover:bg-blue-50 text-xs text-slate-600 flex flex-col items-center gap-1">
                <Package size={16} className="text-blue-500"/> Admin
              </button>
              <button onClick={() => onLogin('delivery', 'Demo Moto')} className="p-2 border border-slate-200 rounded hover:bg-green-50 text-xs text-slate-600 flex flex-col items-center gap-1">
                <Truck size={16} className="text-green-500"/> Moto
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

// 7. Seller Dashboard (UPDATED - New Cart View Integration)
const SellerDashboard = ({ products, onCreateOrder, cart, setCart, onLogout, currentUser, allOrders }) => {
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedProduct, setSelectedProduct] = useState(null);
  
  // Dashboard View State Machine: 'catalog' | 'history' | 'success' | 'product_detail' | 'order_detail' | 'cart'
  const [dashboardView, setDashboardView] = useState('catalog');
  const [lastCreatedOrder, setLastCreatedOrder] = useState(null);
  const [selectedOrder, setSelectedOrder] = useState(null); 

  // History Search State
  const [historySearchTerm, setHistorySearchTerm] = useState('');

  // Category Tree State
  const [selectedCategory, setSelectedCategory] = useState('Todos');

  // Filter logic (Catalog)
  const filteredProducts = products.filter(p => {
    const matchesSearch = p.name.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesCategory = selectedCategory === 'Todos' || p.category === selectedCategory;
    return matchesSearch && matchesCategory;
  });

  // Filter logic (History)
  const filteredOrders = allOrders
    .filter(o => o.seller === currentUser)
    .filter(o => {
      const term = historySearchTerm.toLowerCase();
      return (
        o.id.toString().includes(term) ||
        o.client.toLowerCase().includes(term) ||
        o.ruc.toLowerCase().includes(term)
      );
    })
    .sort((a,b) => b.timestamp - a.timestamp);

  const totalStock = (stock) => stock.central + stock.capuchi + stock.family;

  const addToCart = (product) => {
    if (totalStock(product.stock) > 0) {
      setCart([...cart, product]);
      alert("Agregado al carrito"); 
    } else {
      alert("Producto Agotado");
    }
  };

  // Helper to remove all items of a certain ID or remove just one
  const handleRemoveAll = (itemId) => {
    const newCart = cart.filter(item => item.id !== itemId);
    setCart(newCart);
  };

  const removeFromCart = (index) => {
    const newCart = [...cart];
    newCart.splice(index, 1);
    setCart(newCart);
  };

  const handleConfirmOrder = (orderData) => {
    const newOrder = {
      id: Math.floor(100000 + Math.random() * 900000), 
      client: orderData.name,
      ruc: orderData.ruc || 'Sin RUC',
      phone: orderData.phone, // Include Phone
      items: cart,
      zone: orderData.zone,
      total: orderData.total,
      status: 'pending',
      timestamp: new Date(),
      seller: currentUser || 'Vendedor Externo',
      timeLeft: 900
    };
    
    onCreateOrder(newOrder);
    setCart([]);
    setLastCreatedOrder(newOrder);
    setDashboardView('success');
  };

  const handleProductClick = (product) => {
    setSelectedProduct(product);
    setDashboardView('product_detail');
  };

  const handleOrderClick = (order) => {
    setSelectedOrder(order);
    setDashboardView('order_detail');
  };

  return (
    <div className="min-h-screen bg-slate-50 flex flex-col">
      {/* Header */}
      <header className="bg-slate-900 text-white sticky top-0 z-50 shadow-md">
        <div className="max-w-7xl mx-auto px-4 py-3 flex flex-wrap gap-4 justify-between items-center">
          {/* Logo */}
          <div className="flex items-center gap-2 cursor-pointer flex-shrink-0" onClick={() => setDashboardView('catalog')}>
            <div className="bg-orange-500 p-1.5 rounded text-white font-bold text-lg">FC</div>
            <span className="font-semibold text-lg hidden lg:block">Family Center</span>
          </div>
          
          {/* Main Search Bar (Expanded - Only on Catalog) */}
          {dashboardView === 'catalog' && (
            <div className="order-3 sm:order-2 w-full sm:w-auto flex-1 max-w-2xl min-w-[200px]">
              <div className="relative">
                <input 
                  type="text" 
                  placeholder="Buscar productos, marcas, modelos..." 
                  className="w-full bg-slate-800 text-white px-5 py-3 rounded-xl focus:outline-none focus:ring-2 focus:ring-orange-500 pl-12 text-sm sm:text-base shadow-inner transition-all"
                  value={searchTerm}
                  onChange={(e) => setSearchTerm(e.target.value)}
                />
                <Search className="absolute left-4 top-3.5 text-slate-400" size={20} />
              </div>
            </div>
          )}

          {/* Right Actions */}
          <div className="order-2 sm:order-3 flex items-center gap-2 flex-shrink-0">
            <div className="flex bg-slate-800 rounded-lg p-1">
               <button 
                onClick={() => setDashboardView('catalog')}
                className={`p-2 rounded-md transition-all ${dashboardView === 'catalog' ? 'bg-slate-700 text-white shadow-sm' : 'text-slate-400 hover:text-white'}`}
                title="Catálogo"
               >
                 <Store size={20} />
               </button>
               <button 
                onClick={() => setDashboardView('history')}
                className={`p-2 rounded-md transition-all ${dashboardView === 'history' ? 'bg-slate-700 text-white shadow-sm' : 'text-slate-400 hover:text-white'}`}
                title="Mis Pedidos"
               >
                 <List size={20} />
               </button>
            </div>
            
            <button 
              className={`relative p-2 rounded-full transition-colors ${dashboardView === 'cart' ? 'bg-orange-500 text-white' : 'hover:bg-slate-800'}`}
              onClick={() => setDashboardView('cart')}
            >
              <ShoppingCart size={24} />
              {cart.length > 0 && (
                <span className="absolute top-0 right-0 bg-orange-500 text-white text-xs font-bold w-5 h-5 rounded-full flex items-center justify-center animate-bounce border border-slate-900">
                  {cart.length}
                </span>
              )}
            </button>
            
            <div className="hidden md:flex items-center gap-2 ml-2 border-l border-slate-700 pl-4">
              <div className="text-right">
                <p className="text-xs text-slate-400">Hola,</p>
                <p className="text-sm font-bold leading-none">{currentUser}</p>
              </div>
              <button onClick={onLogout} className="p-2 hover:bg-slate-800 rounded-full text-slate-400 hover:text-white" title="Cerrar Sesión">
                <LogOut size={20} />
              </button>
            </div>
          </div>
        </div>
      </header>

      {/* Main Content Area */}
      <main className="flex-1 p-4 max-w-7xl mx-auto w-full">
        
        {/* VIEW 1: CATALOG */}
        {dashboardView === 'catalog' && (
          <div className="space-y-6">
            <CategorySelector 
              selectedCategory={selectedCategory} 
              onSelectCategory={setSelectedCategory} 
            />
            <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
              {filteredProducts.length === 0 ? (
                <div className="col-span-full text-center py-20 text-slate-400">
                  <Search size={48} className="mx-auto mb-4 opacity-30" />
                  <p className="text-lg">No se encontraron productos en esta categoría.</p>
                </div>
              ) : (
                filteredProducts.map(product => {
                  const stock = totalStock(product.stock);
                  return (
                    <div 
                      key={product.id} 
                      onClick={() => handleProductClick(product)}
                      className="bg-white rounded-xl shadow-sm hover:shadow-xl transition-all cursor-pointer overflow-hidden border border-slate-100 flex flex-col group transform hover:-translate-y-1"
                    >
                      <div className="h-48 bg-slate-100 flex items-center justify-center relative">
                        <Smartphone size={48} className="text-slate-300 group-hover:scale-110 transition-transform duration-300" />
                        <div className="absolute top-2 right-2 flex flex-col gap-1 items-end">
                          {stock > 0 ? (
                            <span className="bg-green-100 text-green-800 text-xs font-bold px-2 py-1 rounded-md shadow-sm border border-green-200 flex items-center gap-1">
                              <Check size={10} /> Stock: {stock}
                            </span>
                          ) : (
                            <span className="bg-red-100 text-red-800 text-xs font-bold px-2 py-1 rounded">Agotado</span>
                          )}
                        </div>
                        <span className="absolute bottom-2 left-2 bg-slate-800/80 backdrop-blur text-white text-[10px] font-bold px-2 py-1 rounded">
                          {product.category}
                        </span>
                      </div>
                      <div className="p-4 flex-1 flex flex-col">
                        <h3 className="font-bold text-slate-800 text-lg leading-tight mb-2 line-clamp-2">{product.name}</h3>
                        <div className="mt-auto pt-4 border-t border-slate-100 flex items-center justify-between">
                          <div>
                            <p className="text-slate-400 text-xs">Precio</p>
                            <p className="font-bold text-xl text-slate-900">Gs. {product.price.toLocaleString()}</p>
                          </div>
                          <button 
                            onClick={(e) => {
                              e.stopPropagation();
                              addToCart(product);
                            }}
                            disabled={stock === 0}
                            className={`p-3 rounded-xl transition-colors ${stock > 0 ? 'bg-slate-100 text-slate-800 hover:bg-slate-900 hover:text-white' : 'bg-slate-50 text-slate-300 cursor-not-allowed'}`}
                          >
                            <ShoppingCart size={20} />
                          </button>
                        </div>
                      </div>
                    </div>
                  );
                })
              )}
            </div>
          </div>
        )}

        {/* VIEW 4: PRODUCT DETAIL */}
        {dashboardView === 'product_detail' && selectedProduct && (
          <ProductDetailView 
            product={selectedProduct} 
            onBack={() => setDashboardView('catalog')}
            onAddToCart={addToCart}
          />
        )}

        {/* VIEW 5: ORDER DETAIL */}
        {dashboardView === 'order_detail' && selectedOrder && (
          <OrderDetailView 
            order={selectedOrder} 
            onBack={() => setDashboardView('history')}
          />
        )}

        {/* VIEW 6: CART CHECKOUT (NEW) */}
        {dashboardView === 'cart' && (
          <CartCheckoutView
            cart={cart}
            onRemoveFromCart={handleRemoveAll}
            onAddOne={addToCart}
            onRemoveOne={(id) => {
               // Find index of first item with this ID
               const idx = cart.findIndex(p => p.id === id);
               if(idx !== -1) removeFromCart(idx);
            }}
            onBackToCatalog={() => setDashboardView('catalog')}
            onConfirmOrder={handleConfirmOrder}
          />
        )}

        {/* VIEW 2: SUCCESS SCREEN */}
        {dashboardView === 'success' && lastCreatedOrder && (
          <div className="flex flex-col items-center justify-center min-h-[60vh] animate-in fade-in zoom-in-95 duration-300">
            <div className="bg-white p-8 rounded-2xl shadow-2xl text-center max-w-md w-full border-t-8 border-green-500">
              <div className="mx-auto bg-green-100 w-24 h-24 rounded-full flex items-center justify-center mb-6">
                <CheckCircle size={48} className="text-green-600" />
              </div>
              <h2 className="text-2xl font-bold text-slate-800 mb-2">¡Pedido Generado!</h2>
              <p className="text-slate-500 mb-6">El pedido se ha enviado a preparación correctamente.</p>
              
              <div className="bg-slate-50 p-4 rounded-xl border border-slate-200 mb-8 relative group">
                <p className="text-xs text-slate-400 uppercase tracking-widest font-bold mb-1">Código de Pedido</p>
                <p className="text-4xl font-black text-slate-900 tracking-tight">#{lastCreatedOrder.id}</p>
              </div>

              <div className="space-y-3">
                 <button 
                  onClick={() => setDashboardView('history')}
                  className="w-full bg-white border-2 border-slate-200 text-slate-700 font-bold py-3 rounded-xl hover:bg-slate-50 transition-colors"
                >
                  Ver mis pedidos
                </button>
                <button 
                  onClick={() => setDashboardView('catalog')}
                  className="w-full bg-slate-900 text-white font-bold py-3 rounded-xl hover:bg-slate-800 transition-colors shadow-lg"
                >
                  Volver al Catálogo
                </button>
              </div>
            </div>
          </div>
        )}

        {/* VIEW 3: ORDER HISTORY */}
        {dashboardView === 'history' && (
          <div className="max-w-4xl mx-auto animate-in slide-in-from-bottom-4 duration-300">
            <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
              <div className="flex items-center gap-4">
                <button onClick={() => setDashboardView('catalog')} className="p-2 hover:bg-slate-200 rounded-full transition-colors">
                  <ArrowLeft size={24} className="text-slate-600" />
                </button>
                <h2 className="text-2xl font-bold text-slate-800">Mis Pedidos Generados</h2>
              </div>
              
              <div className="relative w-full md:w-64">
                <input 
                  type="text" 
                  placeholder="Buscar por ID, Cliente o RUC..." 
                  className="w-full bg-white border border-slate-300 text-slate-700 px-4 py-2 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 pl-10 text-sm shadow-sm"
                  value={historySearchTerm}
                  onChange={(e) => setHistorySearchTerm(e.target.value)}
                />
                <Search className="absolute left-3 top-2.5 text-slate-400" size={16} />
              </div>
            </div>

            <div className="space-y-3">
              {filteredOrders.length === 0 ? (
                <div className="text-center py-20 bg-white rounded-xl border border-dashed border-slate-300">
                   <Package size={48} className="mx-auto text-slate-300 mb-4" />
                   <p className="text-slate-500 font-medium">
                     {historySearchTerm ? 'No se encontraron pedidos con ese criterio.' : 'Aún no has generado pedidos.'}
                   </p>
                </div>
              ) : (
                filteredOrders.map(order => (
                  <div 
                    key={order.id} 
                    onClick={() => handleOrderClick(order)} 
                    className="bg-white p-4 rounded-xl shadow-sm border border-slate-200 hover:shadow-md hover:border-orange-200 transition-all cursor-pointer group"
                  >
                    <div className="flex items-center justify-between">
                      <div className="flex items-start gap-4">
                        <div className={`p-3 rounded-full bg-slate-50 border border-slate-100`}>
                          <FileText size={20} className="text-slate-400" />
                        </div>
                        <div>
                          <div className="flex items-center gap-2 mb-1">
                            <span className="font-bold text-slate-800">#{order.id}</span>
                            <span className={`px-2 py-0.5 rounded text-[10px] font-bold uppercase border ${getStatusColor(order.status)}`}>
                              {getStatusLabel(order.status)}
                            </span>
                          </div>
                          <p className="text-sm font-medium text-slate-600">{order.client}</p>
                          <p className="text-xs text-slate-400">{order.timestamp.toLocaleDateString()} • {order.timestamp.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</p>
                        </div>
                      </div>
                      <div className="text-right flex items-center gap-4">
                        <div>
                          <p className="text-xs text-slate-400 uppercase">Total</p>
                          <p className="font-bold text-slate-900">Gs. {order.total.toLocaleString()}</p>
                        </div>
                        <ChevronRight size={20} className="text-slate-300 group-hover:text-orange-500 transition-colors" />
                      </div>
                    </div>
                  </div>
                ))
              )}
            </div>
          </div>
        )}

      </main>
    </div>
  );
};

// 8. Prep & Delivery Dashboards remain unchanged but must be present in App switch
const PrepDashboard = ({ orders, onUpdateStatus, onLogout, currentUser }) => {
  return (
    <div className="min-h-screen bg-slate-100 flex flex-col">
      <header className="bg-white border-b border-slate-200 px-6 py-4 flex justify-between items-center shadow-sm">
        <h1 className="text-xl font-bold text-slate-800 flex items-center gap-2">
          <Package className="text-blue-600" />
          Panel de Preparación
        </h1>
        <div className="flex items-center gap-4">
          <span className="text-sm font-bold text-slate-600">{currentUser}</span>
          <button onClick={onLogout} className="text-slate-500 hover:text-red-500 flex items-center gap-1 text-sm font-medium">
            <LogOut size={16} /> Salir
          </button>
        </div>
      </header>

      <main className="flex-1 p-6 max-w-6xl mx-auto w-full">
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          {/* Column: Pending */}
          <div>
            <h2 className="text-sm font-bold text-slate-500 uppercase mb-4 flex items-center gap-2">
              <Clock size={16} /> Pre-Pedidos (Pendientes)
            </h2>
            <div className="space-y-4">
              {orders.filter(o => o.status === 'pending').length === 0 && (
                <div className="bg-white p-8 rounded-lg text-center text-slate-400 border border-dashed border-slate-300">
                  No hay pedidos pendientes
                </div>
              )}
              {orders.filter(o => o.status === 'pending').map(order => (
                <div key={order.id} className="bg-white p-4 rounded-lg shadow-sm border-l-4 border-orange-500 relative">
                  <div className="absolute top-4 right-4 bg-orange-100 text-orange-700 text-xs font-bold px-2 py-1 rounded-full flex items-center gap-1">
                    <Clock size={12} />
                    {Math.floor(order.timeLeft / 60)} min restantes
                  </div>

                  <h3 className="font-bold text-slate-800">Pedido #{order.id}</h3>
                  <p className="text-sm text-slate-500 mb-1">Vendedor: {order.seller}</p>
                  
                  <div className="bg-slate-50 p-2 rounded border border-slate-100 mb-2">
                    <p className="text-sm font-bold text-slate-700">{order.client}</p>
                    <p className="text-xs text-slate-500 flex items-center gap-1">
                      <FileText size={10} /> RUC: {order.ruc}
                    </p>
                  </div>
                  
                  <div className="my-3 border-t border-b border-slate-100 py-2 space-y-1">
                    {order.items.map((item, i) => (
                      <div key={i} className="text-xs text-slate-600 flex justify-between">
                        <span>• {item.name}</span>
                        <span className="font-mono text-slate-400">{item.category}</span>
                      </div>
                    ))}
                  </div>

                  <div className="flex justify-between items-end mt-4">
                    <div>
                      <p className="text-xs text-slate-400">Destino: {order.zone.name}</p>
                      <p className="text-lg font-bold text-slate-900">Gs. {order.total.toLocaleString()}</p>
                    </div>
                    <div className="flex gap-2">
                      <button 
                        onClick={() => onUpdateStatus(order.id, 'cancelled')}
                        className="px-3 py-2 text-xs font-bold text-red-600 bg-red-50 hover:bg-red-100 rounded-lg border border-red-200 transition-colors"
                      >
                        ANULAR
                      </button>
                      <button 
                        onClick={() => onUpdateStatus(order.id, 'approved')}
                        className="px-3 py-2 text-xs font-bold text-white bg-blue-600 hover:bg-blue-700 rounded-lg shadow-sm shadow-blue-200 transition-colors flex items-center gap-1"
                      >
                        <Printer size={14} />
                        APROBAR (IMPRIMIR)
                      </button>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>

          {/* Column: Approved / Preparing */}
          <div>
            <h2 className="text-sm font-bold text-slate-500 uppercase mb-4 flex items-center gap-2">
              <CheckCircle size={16} /> En Preparación / Listos
            </h2>
            <div className="space-y-4">
              {orders.filter(o => o.status === 'approved').map(order => (
                <div key={order.id} className="bg-slate-50 p-4 rounded-lg border border-slate-200 opacity-75 hover:opacity-100 transition-opacity">
                  <div className="flex justify-between items-start mb-2">
                    <h3 className="font-bold text-slate-700">Pedido #{order.id}</h3>
                    <span className="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full border border-green-200">En Preparación</span>
                  </div>
                  <p className="text-xs text-slate-500">Cliente: {order.client}</p>
                  <p className="text-xs text-slate-500">Zona: {order.zone.name}</p>
                  <div className="mt-3 flex justify-end">
                    <span className="text-xs font-mono text-slate-400">Esperando asignación de moto...</span>
                  </div>
                </div>
              ))}
            </div>
          </div>
        </div>
      </main>
    </div>
  );
};

const DeliveryDashboard = ({ orders, onUpdateStatus, onLogout, currentUser }) => {
  const myOrders = orders.filter(o => o.status === 'approved' || o.status === 'delivering');

  return (
    <div className="min-h-screen bg-slate-100">
      <header className="bg-green-600 text-white px-4 py-4 flex justify-between items-center shadow-lg">
        <h1 className="font-bold text-lg flex items-center gap-2">
          <Truck /> Modo Moto
        </h1>
        <div className="flex items-center gap-2">
          <span className="text-xs font-bold bg-white/20 px-2 py-1 rounded">{currentUser}</span>
          <button onClick={onLogout} className="bg-green-700 p-2 rounded-full"><LogOut size={18} /></button>
        </div>
      </header>
      <main className="p-4 space-y-4">
        {myOrders.length === 0 && (
          <div className="text-center mt-20 text-slate-400">
            <p>No hay entregas pendientes</p>
          </div>
        )}
        {myOrders.map(order => (
          <div key={order.id} className="bg-white rounded-xl shadow-md overflow-hidden">
            <div className="bg-slate-800 text-white p-3 flex justify-between items-center">
              <span className="font-bold">#{order.id}</span>
              <span className="text-xs bg-white/20 px-2 py-1 rounded">Gs. {order.total.toLocaleString()}</span>
            </div>
            <div className="p-4">
              <div className="flex items-start gap-3 mb-4">
                <MapPin className="text-red-500 mt-1" size={20} />
                <div>
                  <p className="font-bold text-slate-800 text-lg">{order.zone.name}</p>
                  <p className="text-slate-500 text-sm">{order.client}</p>
                </div>
              </div>
              <a 
                href={`https://maps.google.com/?q=${order.zone.name}`} 
                target="_blank" 
                rel="noreferrer"
                className="block w-full text-center bg-slate-100 text-slate-600 py-2 rounded-lg text-sm font-bold mb-4 hover:bg-slate-200"
              >
                ABRIR EN MAPS
              </a>
              {order.status === 'approved' ? (
                <button 
                  onClick={() => onUpdateStatus(order.id, 'delivering')}
                  className="w-full bg-green-600 text-white py-3 rounded-lg font-bold shadow-lg shadow-green-200 hover:bg-green-700"
                >
                  RETIRAR PEDIDO
                </button>
              ) : (
                <div className="flex gap-2">
                  <button 
                    onClick={() => onUpdateStatus(order.id, 'cancelled')}
                    className="flex-1 bg-red-100 text-red-600 py-3 rounded-lg font-bold text-xs"
                  >
                    NO ENTREGADO
                  </button>
                  <button 
                    onClick={() => onUpdateStatus(order.id, 'delivered')}
                    className="flex-[2] bg-blue-600 text-white py-3 rounded-lg font-bold shadow-lg shadow-blue-200 hover:bg-blue-700"
                  >
                    ENTREGADO
                  </button>
                </div>
              )}
            </div>
          </div>
        ))}
      </main>
    </div>
  );
};

// --- APP ORCHESTRATOR ---

const App = () => {
  const [view, setView] = useState('login');
  const [userType, setUserType] = useState(null);
  const [username, setUsername] = useState('');
  const [orders, setOrders] = useState([]);
  const [cart, setCart] = useState([]);

  useEffect(() => {
    const timer = setInterval(() => {
      setOrders(currentOrders => 
        currentOrders.map(order => {
          if (order.status === 'pending' && order.timeLeft > 0) {
            return { ...order, timeLeft: order.timeLeft - 1 };
          }
          if (order.status === 'pending' && order.timeLeft === 0) {
            return { ...order, status: 'cancelled_timeout' };
          }
          return order;
        })
      );
    }, 1000);
    return () => clearInterval(timer);
  }, []);

  const handleLogin = (type, name) => {
    setUserType(type);
    setUsername(name);
    setView(type);
  };

  const handleCreateOrder = (newOrder) => {
    setOrders(prevOrders => {
      const updatedPrevOrders = prevOrders.map(o => 
        o.status === 'pending' ? { ...o, status: 'approved' } : o
      );
      return [...updatedPrevOrders, newOrder];
    });
  };

  const handleUpdateStatus = (id, status) => {
    setOrders(orders.map(o => o.id === id ? { ...o, status } : o));
  };

  const handleLogout = () => {
    setUserType(null);
    setUsername('');
    setView('login');
  };

  switch (view) {
    case 'login':
      return <Login onLogin={handleLogin} />;
    case 'seller':
      return (
        <SellerDashboard 
          products={PRODUCTS} 
          onCreateOrder={handleCreateOrder} 
          cart={cart}
          setCart={setCart}
          onLogout={handleLogout}
          currentUser={username}
          allOrders={orders} 
        />
      );
    case 'prep':
      return (
        <PrepDashboard 
          orders={orders} 
          onUpdateStatus={handleUpdateStatus} 
          onLogout={handleLogout}
          currentUser={username}
        />
      );
    case 'delivery':
      return (
        <DeliveryDashboard 
          orders={orders} 
          onUpdateStatus={handleUpdateStatus} 
          onLogout={handleLogout}
          currentUser={username}
        />
      );
    default:
      return <Login onLogin={handleLogin} />;
  }
};

export default App;
