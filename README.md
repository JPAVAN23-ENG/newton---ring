<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Newton's Rings Simulation</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .ring-animation {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(99, 102, 241, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(99, 102, 241, 0); }
            100% { box-shadow: 0 0 0 0 rgba(99, 102, 241, 0); }
        }
        .color-preview {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            display: inline-block;
            margin-right: 8px;
            border: 1px solid #ccc;
        }
        .white-preview {
            background: linear-gradient(45deg, #f3f3f3 25%, #ffffff 25%, #ffffff 50%, #f3f3f3 50%, #f3f3f3 75%, #ffffff 75%, #ffffff 100%);
            background-size: 8px 8px;
        }
        .medium-icon {
            width: 24px;
            height: 24px;
            margin-right: 8px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }
        .oil-preview {
            background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
        }
        .diamond-preview {
            background: linear-gradient(135deg, #e0e0e0 0%, #ffffff 100%);
            position: relative;
            overflow: hidden;
        }
        .diamond-preview::after {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(
                to bottom right,
                rgba(255,255,255,0) 45%,
                rgba(255,255,255,0.8) 50%,
                rgba(255,255,255,0) 55%
            );
            transform: rotate(45deg);
        }
        #ring-pattern {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            opacity: 0.7;
        }
        #ring-pattern div {
            position: absolute;
            border-radius: 50%;
            border-width: 2px;
            border-style: solid;
            transform: translate(-50%, -50%);
            top: 50%;
            left: 50%;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <div class="container mx-auto px-4 py-8">
        <div class="text-center mb-8">
            <h1 class="text-3xl font-bold text-indigo-700 mb-2">Newton's Rings Simulation</h1>
            <p class="text-gray-600 max-w-2xl mx-auto">Explore the interference pattern created by light reflection between curved and flat surfaces with different media</p>
        </div>
        
        <div class="bg-white rounded-xl shadow-xl p-6 mb-8 border border-gray-100">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <!-- Input Section -->
                <div>
                    <h2 class="text-xl font-semibold text-gray-800 mb-4 flex items-center">
                        <i class="fas fa-sliders-h mr-2 text-indigo-600"></i> Simulation Parameters
                    </h2>
                    
                    <div class="mb-6">
                        <label class="block text-gray-700 mb-2 font-medium">Color of Light</label>
                        <div class="grid grid-cols-2 gap-3">
                            <div class="flex items-center bg-gray-50 p-2 rounded-lg">
                                <input type="radio" id="neon" name="color" value="neon" checked class="mr-2">
                                <label for="neon" class="flex items-center cursor-pointer w-full">
                                    <span class="color-preview bg-pink-500"></span>
                                    <span>Neon (Pink)</span>
                                </label>
                            </div>
                            <div class="flex items-center bg-gray-50 p-2 rounded-lg">
                                <input type="radio" id="indigo" name="color" value="indigo" class="mr-2">
                                <label for="indigo" class="flex items-center cursor-pointer w-full">
                                    <span class="color-preview bg-indigo-700"></span>
                                    <span>Indigo</span>
                                </label>
                            </div>
                            <div class="flex items-center bg-gray-50 p-2 rounded-lg">
                                <input type="radio" id="orange" name="color" value="orange" class="mr-2">
                                <label for="orange" class="flex items-center cursor-pointer w-full">
                                    <span class="color-preview bg-orange-500"></span>
                                    <span>Orange</span>
                                </label>
                            </div>
                            <div class="flex items-center bg-gray-50 p-2 rounded-lg">
                                <input type="radio" id="white" name="color" value="white" class="mr-2">
                                <label for="white" class="flex items-center cursor-pointer w-full">
                                    <span class="color-preview white-preview"></span>
                                    <span>White</span>
                                </label>
                            </div>
                        </div>
                    </div>
                    
                    <div class="mb-6">
                        <label class="block text-gray-700 mb-2 font-medium">Medium</label>
                        <select id="medium" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 bg-gray-50">
                            <option value="air">
                                <span class="medium-icon"><i class="fas fa-wind"></i></span>
                                Air (n=1.0003)
                            </option>
                            <option value="water">
                                <span class="medium-icon"><i class="fas fa-tint"></i></span>
                                Water (n=1.333)
                            </option>
                            <option value="oil">
                                <span class="medium-icon"><i class="fas fa-oil-can"></i></span>
                                Oil (n=1.45)
                            </option>
                            <option value="glass">
                                <span class="medium-icon"><i class="fas fa-glass-whiskey"></i></span>
                                Glass (n=1.5)
                            </option>
                            <option value="diamond">
                                <span class="medium-icon"><i class="far fa-gem"></i></span>
                                Diamond (n=2.42)
                            </option>
                        </select>
                    </div>
                    
                    <div class="mb-6">
                        <label for="radius" class="block text-gray-700 mb-2 font-medium">Radius of Curvature (cm)</label>
                        <div class="flex items-center">
                            <input type="range" id="radius-slider" min="10" max="500" value="100" class="w-full mr-4">
                            <input type="number" id="radius" value="100" min="10" max="500" step="1" 
                                   class="w-24 p-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500">
                        </div>
                    </div>
                    
                    <div class="mb-6">
                        <label for="rings" class="block text-gray-700 mb-2 font-medium">Number of Rings to Calculate</label>
                        <div class="flex items-center">
                            <input type="range" id="rings-slider" min="1" max="20" value="5" class="w-full mr-4">
                            <input type="number" id="rings" value="5" min="1" max="20" 
                                   class="w-24 p-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500">
                        </div>
                    </div>
                    
                    <button id="calculate" class="w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-[1.02] shadow-md flex items-center justify-center">
                        <i class="fas fa-calculator mr-2"></i> Calculate Newton's Rings
                    </button>
                </div>
                
                <!-- Visualization Section -->
                <div class="flex flex-col items-center justify-center">
                    <div class="relative w-72 h-72 mb-6">
                        <div id="newtons-ring" class="absolute inset-0 rounded-full border-4 border-indigo-400 ring-animation bg-gray-100 flex items-center justify-center overflow-hidden">
                            <div id="ring-pattern"></div>
                            <div class="text-center px-4 z-10 bg-white bg-opacity-80 rounded-lg p-2">
                                <p class="text-gray-800 font-medium">Newton's Rings</p>
                                <p id="current-color" class="text-sm text-pink-500 font-medium">Neon (Pink)</p>
                                <p id="current-medium" class="text-sm text-gray-700">Air (n=1.0003)</p>
                            </div>
                        </div>
                    </div>
                    <div class="text-center bg-indigo-50 rounded-lg p-3 w-full">
                        <p class="text-indigo-800 font-medium"><i class="fas fa-info-circle mr-2"></i>Visual representation of Newton's rings pattern</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Results Section -->
        <div id="results" class="bg-white rounded-xl shadow-xl p-6 hidden border border-gray-100">
            <h2 class="text-xl font-semibold text-gray-800 mb-4 flex items-center">
                <i class="fas fa-chart-bar mr-2 text-indigo-600"></i> Calculation Results
            </h2>
            
            <div class="mb-6 bg-gray-50 p-4 rounded-lg">
                <div class="flex flex-wrap items-center gap-4">
                    <div class="flex items-center">
                        <span class="color-preview" id="result-color-preview"></span>
                        <span id="result-color-text" class="font-medium"></span>
                    </div>
                    <div class="flex items-center">
                        <span class="medium-icon" id="result-medium-icon"></span>
                        <span id="result-medium-text" class="font-medium"></span>
                    </div>
                    <div class="flex items-center">
                        <i class="fas fa-ruler-combined mr-2 text-gray-600"></i>
                        <span id="result-radius-text" class="font-medium"></span>
                    </div>
                </div>
            </div>
            
            <div class="overflow-x-auto rounded-lg border border-gray-200 shadow-sm">
                <table class="min-w-full bg-white">
                    <thead class="bg-gray-100">
                        <tr>
                            <th class="py-3 px-4 border-b font-semibold text-left">Ring</th>
                            <th class="py-3 px-4 border-b font-semibold text-left">Wavelength</th>
                            <th class="py-3 px-4 border-b font-semibold text-left">Diameter</th>
                            <th class="py-3 px-4 border-b font-semibold text-left">Thickness</th>
                            <th class="py-3 px-4 border-b font-semibold text-left">Path Diff.</th>
                        </tr>
                    </thead>
                    <tbody id="results-table" class="divide-y divide-gray-200">
                        <!-- Results will be inserted here -->
                    </tbody>
                </table>
            </div>
            
            <div class="mt-6 flex flex-wrap justify-between items-center gap-4">
                <button id="export-csv" class="bg-gradient-to-r from-green-600 to-emerald-600 hover:from-green-700 hover:to-emerald-700 text-white font-bold py-2 px-4 rounded-lg flex items-center">
                    <i class="fas fa-file-csv mr-2"></i>Export to CSV
                </button>
                <button id="export-png" class="bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-700 hover:to-cyan-700 text-white font-bold py-2 px-4 rounded-lg flex items-center">
                    <i class="fas fa-image mr-2"></i>Export as PNG
                </button>
                <button id="reset" class="bg-gradient-to-r from-gray-600 to-gray-700 hover:from-gray-700 hover:to-gray-800 text-white font-bold py-2 px-4 rounded-lg flex items-center">
                    <i class="fas fa-redo mr-2"></i>Reset Simulation
                </button>
            </div>
        </div>
        
        <!-- Theory Section -->
        <div class="bg-white rounded-xl shadow-xl p-6 mt-8 border border-gray-100">
            <h2 class="text-xl font-semibold text-gray-800 mb-4 flex items-center">
                <i class="fas fa-book-open mr-2 text-indigo-600"></i> Theory Behind Newton's Rings
            </h2>
            <div class="prose max-w-none">
                <p class="mb-4 text-gray-700">Newton's rings is an interference pattern caused by the reflection of light between two surfaces—a spherical surface and an adjacent flat surface. When viewed with monochromatic light, it appears as a series of concentric, alternating bright and dark rings.</p>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                    <!-- Key Formulas -->
                    <div class="bg-indigo-50 p-4 rounded-lg">
                        <h3 class="text-lg font-semibold text-indigo-800 mb-3 flex items-center">
                            <i class="fas fa-square-root-alt mr-2"></i> Key Formulas
                        </h3>
                        <div class="space-y-3">
                            <div>
                                <p class="font-medium text-gray-800">Wavelength of light:</p>
                                <div class="bg-white p-2 rounded font-mono text-center my-1">
                                    λ = c / f
                                </div>
                                <p class="text-sm text-gray-600">where c is speed of light, f is frequency</p>
                            </div>
                            <div>
                                <p class="font-medium text-gray-800">Diameter of m<sup>th</sup> dark ring:</p>
                                <div class="bg-white p-2 rounded font-mono text-center my-1">
                                    D<sub>m</sub> = 2√(mλR/μ)
                                </div>
                            </div>
                            <div>
                                <p class="font-medium text-gray-800">Thickness at m<sup>th</sup> ring:</p>
                                <div class="bg-white p-2 rounded font-mono text-center my-1">
                                    t = (r<sub>m</sub>)² / (2R)
                                </div>
                            </div>
                            <div>
                                <p class="font-medium text-gray-800">Path difference:</p>
                                <div class="bg-white p-2 rounded font-mono text-center my-1">
                                    2μt + λ/2
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Variables -->
                    <div class="bg-indigo-50 p-4 rounded-lg">
                        <h3 class="text-lg font-semibold text-indigo-800 mb-3 flex items-center">
                            <i class="fas fa-variable mr-2"></i> Variables
                        </h3>
                        <ul class="list-disc pl-5 space-y-1 text-gray-700">
                            <li><strong>D<sub>m</sub></strong>: Diameter of m<sup>th</sup> dark ring</li>
                            <li><strong>m</strong>: Ring number (1, 2, 3...)</li>
                            <li><strong>λ</strong>: Wavelength of light (nm)</li>
                            <li><strong>c</strong>: Speed of light (3×10⁸ m/s)</li>
                            <li><strong>f</strong>: Frequency of light (Hz)</li>
                            <li><strong>R</strong>: Radius of curvature of lens (cm)</li>
                            <li><strong>μ</strong>: Refractive index of medium</li>
                            <li><strong>t</strong>: Thickness of air film (μm)</li>
                            <li><strong>r<sub>m</sub></strong>: Radius of m<sup>th</sup> ring</li>
                        </ul>
                    </div>
                </div>
                
                <!-- Color Wavelengths -->
                <div class="mb-6">
                    <h3 class="text-lg font-semibold text-gray-800 mb-3 flex items-center">
                        <i class="fas fa-palette mr-2 text-indigo-600"></i> Color Wavelengths
                    </h3>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
                        <div class="bg-pink-50 p-3 rounded-lg flex items-center">
                            <span class="color-preview bg-pink-500 mr-3"></span>
                            <div>
                                <p class="font-medium">Neon (Pink)</p>
                                <p class="text-sm text-gray-600">~632.8 nm</p>
                            </div>
                        </div>
                        <div class="bg-indigo-50 p-3 rounded-lg flex items-center">
                            <span class="color-preview bg-indigo-700 mr-3"></span>
                            <div>
                                <p class="font-medium">Indigo</p>
                                <p class="text-sm text-gray-600">~445 nm</p>
                            </div>
                        </div>
                        <div class="bg-orange-50 p-3 rounded-lg flex items-center">
                            <span class="color-preview bg-orange-500 mr-3"></span>
                            <div>
                                <p class="font-medium">Orange</p>
                                <p class="text-sm text-gray-600">~600 nm</p>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-lg flex items-center">
                            <span class="color-preview white-preview mr-3"></span>
                            <div>
                                <p class="font-medium">White</p>
                                <p class="text-sm text-gray-600">400-700 nm</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Medium Refractive Indices -->
                <div>
                    <h3 class="text-lg font-semibold text-gray-800 mb-3 flex items-center">
                        <i class="fas fa-flask mr-2 text-indigo-600"></i> Medium Refractive Indices
                    </h3>
                    <div class="grid grid-cols-2 md:grid-cols-5 gap-3">
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <div class="flex items-center mb-2">
                                <span class="medium-icon"><i class="fas fa-wind"></i></span>
                                <span class="font-medium">Air</span>
                            </div>
                            <p class="text-sm text-gray-600">n ≈ 1.0003</p>
                        </div>
                        <div class="bg-blue-50 p-3 rounded-lg">
                            <div class="flex items-center mb-2">
                                <span class="medium-icon"><i class="fas fa-tint"></i></span>
                                <span class="font-medium">Water</span>
                            </div>
                            <p class="text-sm text-gray-600">n ≈ 1.333</p>
                        </div>
                        <div class="bg-yellow-50 p-3 rounded-lg">
                            <div class="flex items-center mb-2">
                                <span class="medium-icon"><i class="fas fa-oil-can"></i></span>
                                <span class="font-medium">Oil</span>
                            </div>
                            <p class="text-sm text-gray-600">n ≈ 1.45</p>
                        </div>
                        <div class="bg-purple-50 p-3 rounded-lg">
                            <div class="flex items-center mb-2">
                                <span class="medium-icon"><i class="fas fa-glass-whiskey"></i></span>
                                <span class="font-medium">Glass</span>
                            </div>
                            <p class="text-sm text-gray-600">n ≈ 1.5</p>
                        </div>
                        <div class="bg-gray-100 p-3 rounded-lg">
                            <div class="flex items-center mb-2">
                                <span class="medium-icon"><i class="far fa-gem"></i></span>
                                <span class="font-medium">Diamond</span>
                            </div>
                            <p class="text-sm text-gray-600">n ≈ 2.42</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Color and medium data
            const colorData = {
                neon: { name: "Neon (Pink)", wavelength: 632.8, colorClass: "bg-pink-500", textClass: "text-pink-500" },
                indigo: { name: "Indigo", wavelength: 445, colorClass: "bg-indigo-700", textClass: "text-indigo-700" },
                orange: { name: "Orange", wavelength: 600, colorClass: "bg-orange-500", textClass: "text-orange-500" },
                white: { name: "White", wavelength: 550, colorClass: "white-preview", textClass: "text-gray-700" }
            };
            
            const mediumData = {
                air: { name: "Air", refractiveIndex: 1.0003, icon: '<i class="fas fa-wind"></i>' },
                water: { name: "Water", refractiveIndex: 1.333, icon: '<i class="fas fa-tint"></i>' },
                oil: { name: "Oil", refractiveIndex: 1.45, icon: '<i class="fas fa-oil-can"></i>' },
                glass: { name: "Glass", refractiveIndex: 1.5, icon: '<i class="fas fa-glass-whiskey"></i>' },
                diamond: { name: "Diamond", refractiveIndex: 2.42, icon: '<i class="far fa-gem"></i>' }
            };
            
            // DOM elements
            const calculateBtn = document.getElementById('calculate');
            const resultsSection = document.getElementById('results');
            const resultsTable = document.getElementById('results-table');
            const currentColorText = document.getElementById('current-color');
            const currentMediumText = document.getElementById('current-medium');
            const resultColorPreview = document.getElementById('result-color-preview');
            const resultColorText = document.getElementById('result-color-text');
            const resultMediumIcon = document.getElementById('result-medium-icon');
            const resultMediumText = document.getElementById('result-medium-text');
            const resultRadiusText = document.getElementById('result-radius-text');
            const exportCsvBtn = document.getElementById('export-csv');
            const exportPngBtn = document.getElementById('export-png');
            const resetBtn = document.getElementById('reset');
            const newtonsRing = document.getElementById('newtons-ring');
            const ringPattern = document.getElementById('ring-pattern');
            const radiusSlider = document.getElementById('radius-slider');
            const radiusInput = document.getElementById('radius');
            const ringsSlider = document.getElementById('rings-slider');
            const ringsInput = document.getElementById('rings');
            
            // Sync sliders with inputs
            radiusSlider.addEventListener('input', () => {
                radiusInput.value = radiusSlider.value;
                updateVisualization();
            });
            
            radiusInput.addEventListener('input', () => {
                radiusSlider.value = radiusInput.value;
                updateVisualization();
            });
            
            ringsSlider.addEventListener('input', () => {
                ringsInput.value = ringsSlider.value;
                updateVisualization();
            });
            
            ringsInput.addEventListener('input', () => {
                ringsSlider.value = ringsInput.value;
                updateVisualization();
            });
            
            // Update visualization when color or medium changes
            document.querySelectorAll('input[name="color"]').forEach(radio => {
                radio.addEventListener('change', updateVisualization);
            });
            
            document.getElementById('medium').addEventListener('change', updateVisualization);
            
            function updateVisualization() {
                const selectedColor = document.querySelector('input[name="color"]:checked').value;
                const selectedMedium = document.getElementById('medium').value;
                const numRings = parseInt(ringsInput.value);
                
                const color = colorData[selectedColor];
                const medium = mediumData[selectedMedium];
                
                // Update text
                currentColorText.textContent = color.name;
                currentColorText.className = text-sm ${color.textClass} font-medium;
                currentMediumText.textContent = ${medium.name} (n=${medium.refractiveIndex});
                
                // Update ring color
                newtonsRing.classList.remove('border-pink-500', 'border-indigo-700', 'border-orange-500', 'border-white');
                
                if (selectedColor === 'white') {
                    newtonsRing.classList.add('border-white');
                    newtonsRing.style.borderColor = 'rgba(255,255,255,0.8)';
                } else {
                    newtonsRing.classList.add(border-${selectedColor === 'neon' ? 'pink' : selectedColor}-500);
                    newtonsRing.style.borderColor = '';
                }
                
                // Update ring pattern preview
                updateRingPatternPreview();
            }
            
            function updateRingPatternPreview() {
                const selectedColor = document.querySelector('input[name="color"]:checked').value;
                const radius = parseFloat(radiusInput.value);
                const numRings = parseInt(ringsInput.value);
                
                // Clear previous pattern
                ringPattern.innerHTML = '';
                
                // Create concentric circles for the pattern
                const containerSize = 288; // 72 * 4 (72 is the width in rem)
                const center = containerSize / 2;
                
                for (let m = 1; m <= numRings; m++) {
                    const ring = document.createElement('div');
                    
                    // Calculate diameter based on ring number
                    const diameterRatio = Math.sqrt(m / numRings);
                    const diameter = diameterRatio * containerSize * 0.9;
                    
                    // Position the ring
                    ring.style.width = ${diameter}px;
                    ring.style.height = ${diameter}px;
                    ring.style.left = '50%';
                    ring.style.top = '50%';
                    ring.style.transform = 'translate(-50%, -50%)';
                    
                    // Alternate colors for dark and bright rings
                    if (selectedColor === 'white') {
                        // For white light, create rainbow rings
                        const hue = (m / numRings) * 360;
                        ring.style.borderColor = hsl(${hue}, 80%, 60%);
                    } else {
                        if (m % 2 === 0) {
                            // Bright ring
                            ring.style.borderColor = colorData[selectedColor].textClass === 'text-pink-500' ? 
                                'rgba(236, 72, 153, 0.7)' : 
                                colorData[selectedColor].textClass === 'text-indigo-700' ?
                                'rgba(79, 70, 229, 0.7)' :
                                'rgba(249, 115, 22, 0.7)';
                        } else {
                            // Dark ring
                            ring.style.borderColor = 'rgba(0, 0, 0, 0.5)';
                        }
                    }
                    
                    ring.style.borderWidth = '2px';
                    ring.style.borderStyle = 'solid';
                    ring.style.borderRadius = '50%';
                    
                    ringPattern.appendChild(ring);
                }
            }
            
            // Calculate button click handler
            calculateBtn.addEventListener('click', function() {
                const selectedColor = document.querySelector('input[name="color"]:checked').value;
                const selectedMedium = document.getElementById('medium').value;
                const radius = parseFloat(radiusInput.value);
                const numRings = parseInt(ringsInput.value);
                
                const color = colorData[selectedColor];
                const medium = mediumData[selectedMedium];
                const wavelength = color.wavelength; // in nm
                const refractiveIndex = medium.refractiveIndex;
                
                // Update results header
                resultColorPreview.className = color-preview ${color.colorClass};
                resultColorText.textContent = color.name;
                resultMediumIcon.innerHTML = medium.icon;
                resultMediumText.textContent = ${medium.name} (n=${medium.refractiveIndex});
                resultRadiusText.textContent = R = ${radius} cm;
                
                // Clear previous results
                resultsTable.innerHTML = '';
                
                // Special handling for white light (show multiple wavelengths)
                if (selectedColor === 'white') {
                    // Show results for multiple wavelengths (rainbow colors)
                    const whiteLightWavelengths = [
                        { name: "Violet", wavelength: 400, colorClass: "bg-violet-500" },
                        { name: "Indigo", wavelength: 445, colorClass: "bg-indigo-700" },
                        { name: "Blue", wavelength: 475, colorClass: "bg-blue-500" },
                        { name: "Green", wavelength: 510, colorClass: "bg-green-500" },
                        { name: "Yellow", wavelength: 570, colorClass: "bg-yellow-500" },
                        { name: "Orange", wavelength: 600, colorClass: "bg-orange-500" },
                        { name: "Red", wavelength: 650, colorClass: "bg-red-500" }
                    ];
                    
                    // Add header for white light section
                    const whiteHeader = document.createElement('tr');
                    whiteHeader.innerHTML = `
                        <td colspan="5" class="py-3 px-4 border-b text-center font-bold bg-gray-100">
                            White Light Components (Multiple Wavelengths)
                        </td>
                    `;
                    resultsTable.appendChild(whiteHeader);
                    
                    // Calculate for each wavelength in white light
                    whiteLightWavelengths.forEach(wl => {
                        const wavelengthHeader = document.createElement('tr');
                        wavelengthHeader.innerHTML = `
                            <td colspan="5" class="py-2 px-4 border-b text-center font-medium" style="background-color: ${
                                wl.colorClass.includes('violet') ? 'rgba(148, 0, 211, 0.1)' :
                                wl.colorClass.includes('indigo') ? 'rgba(75, 0, 130, 0.1)' :
                                wl.colorClass.includes('blue') ? 'rgba(0, 0, 255, 0.1)' :
                                wl.colorClass.includes('green') ? 'rgba(0, 128, 0, 0.1)' :
                                wl.colorClass.includes('yellow') ? 'rgba(255, 255, 0, 0.1)' :
                                wl.colorClass.includes('orange') ? 'rgba(255, 165, 0, 0.1)' :
                                'rgba(255, 0, 0, 0.1)'
                            }">
                                ${wl.name} (${wl.wavelength} nm)
                            </td>
                        `;
                        resultsTable.appendChild(wavelengthHeader);
                        
                        // Calculate for each ring
                        for (let m = 1; m <= numRings; m++) {
                            const wavelengthCm = wl.wavelength * 1e-7;
                            const diameter = 2 * Math.sqrt(m * wavelengthCm * radius / refractiveIndex);
                            const radiusCm = diameter / 2;
                            const thickness = (radiusCm * radiusCm) / (2 * radius) * 1e4; // convert to μm
                            const pathDifference = (2 * refractiveIndex * (thickness * 1e-4)) + (wl.wavelength / 2);
                            
                            const row = document.createElement('tr');
                            row.className = m % 2 === 0 ? 'bg-gray-50' : '';
                            row.innerHTML = `
                                <td class="py-2 px-4 border-b">${m}</td>
                                <td class="py-2 px-4 border-b">${wl.wavelength.toFixed(2)} nm</td>
                                <td class="py-2 px-4 border-b">${diameter.toFixed(4)} cm</td>
                                <td class="py-2 px-4 border-b">${thickness.toFixed(2)} μm</td>
                                <td class="py-2 px-4 border-b">${pathDifference.toFixed(2)} nm</td>
                            `;
                            resultsTable.appendChild(row);
                        }
                    });
                } else {
                    // Regular calculation for single wavelength
                    for (let m = 1; m <= numRings; m++) {
                        // Calculate diameter in cm
                        // Formula: D = 2 * sqrt(m * λ * R / μ)
                        // λ needs to be in cm (convert from nm: 1nm = 1e-7 cm)
                        const wavelengthCm = wavelength * 1e-7;
                        const diameter = 2 * Math.sqrt(m * wavelengthCm * radius / refractiveIndex);
                        
                        // Calculate thickness at the edge of the ring (t = r²/2R)
                        const radiusCm = diameter / 2;
                        const thickness = (radiusCm * radiusCm) / (2 * radius) * 1e4; // convert to μm
                        
                        // Calculate path difference
                        const pathDifference = (2 * refractiveIndex * (thickness * 1e-4)) + (wavelength / 2);
                        
                        // Create table row
                        const row = document.createElement('tr');
                        row.className = m % 2 === 0 ? 'bg-gray-50' : '';
                        row.innerHTML = `
                            <td class="py-2 px-4 border-b">${m}</td>
                            <td class="py-2 px-4 border-b">${wavelength.toFixed(2)} nm</td>
                            <td class="py-2 px-4 border-b">${diameter.toFixed(4)} cm</td>
                            <td class="py-2 px-4 border-b">${thickness.toFixed(2)} μm</td>
                            <td class="py-2 px-4 border-b">${pathDifference.toFixed(2)} nm</td>
                        `;
                        resultsTable.appendChild(row);
                    }
                }
                
                // Show results section
                resultsSection.classList.remove('hidden');
                
                // Scroll to results
                resultsSection.scrollIntoView({ behavior: 'smooth' });
            });
            
            // Export to CSV
            exportCsvBtn.addEventListener('click', function() {
                const selectedColor = document.querySelector('input[name="color"]:checked').value;
                const selectedMedium = document.getElementById('medium').value;
                const radius = radiusInput.value;
                const numRings = ringsInput.value;
                
                const color = colorData[selectedColor];
                const medium = mediumData[selectedMedium];
                
                let csvContent = "data:text/csv;charset=utf-8,";
                
                // Add header
                csvContent += "Newton's Rings Simulation Results\r\n";
                csvContent += Color: ${color.name}, Medium: ${medium.name}, Radius: ${radius} cm\r\n\r\n;
                
                // Special handling for white light
                if (selectedColor === 'white') {
                    csvContent += "Wavelength (nm),Ring Number (m),Diameter (cm),Thickness (μm),Path Difference (nm)\r\n";
                    
                    const whiteLightWavelengths = [
                        { name: "Violet", wavelength: 400 },
                        { name: "Indigo", wavelength: 445 },
                        { name: "Blue", wavelength: 475 },
                        { name: "Green", wavelength: 510 },
                        { name: "Yellow", wavelength: 570 },
                        { name: "Orange", wavelength: 600 },
                        { name: "Red", wavelength: 650 }
                    ];
                    
                    whiteLightWavelengths.forEach(wl => {
                        for (let m = 1; m <= numRings; m++) {
                            const wavelengthCm = wl.wavelength * 1e-7;
                            const diameter = 2 * Math.sqrt(m * wavelengthCm * radius / medium.refractiveIndex);
                            const radiusCm = diameter / 2;
                            const thickness = (radiusCm * radiusCm) / (2 * radius) * 1e4;
                            const pathDifference = (2 * medium.refractiveIndex * (thickness * 1e-4)) + (wl.wavelength / 2);
                            
                            csvContent += ${wl.wavelength},${m},${diameter.toFixed(4)},${thickness.toFixed(2)},${pathDifference.toFixed(2)}\r\n;
                        }
                    });
                } else {
                    csvContent += "Ring Number (m),Wavelength (nm),Diameter (cm),Thickness (μm),Path Difference (nm)\r\n";
                    
                    const rows = document.querySelectorAll('#results-table tr');
                    rows.forEach(row => {
                        const cols = row.querySelectorAll('td');
                        if (cols.length === 5) {
                            csvContent += ${cols[0].textContent},${cols[1].textContent},${cols[2].textContent},${cols[3].textContent},${cols[4].textContent}\r\n;
                        }
                    });
                }
                
                // Create download link
                const encodedUri = encodeURI(csvContent);
                const link = document.createElement("a");
                link.setAttribute("href", encodedUri);
                link.setAttribute("download", "newtons_rings_results.csv");
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            });
            
            // Export to PNG (simplified version - would need html2canvas for real implementation)
            exportPngBtn.addEventListener('click', function() {
                alert("In a real implementation, this would use a library like html2canvas to capture the visualization as an image. For this demo, it's just a placeholder.");
            });
            
            // Reset button
            resetBtn.addEventListener('click', function() {
                radiusInput.value = 100;
                radiusSlider.value = 100;
                ringsInput.value = 5;
                ringsSlider.value = 5;
                document.getElementById('neon').checked = true;
                document.getElementById('medium').value = 'air';
                resultsSection.classList.add('hidden');
                updateVisualization();
            });
            
            // Initialize visualization
            updateVisualization();
        });
    </script>
</body>
</html>
