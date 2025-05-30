<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WBS Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #2c3e50, #34495e);
            color: white;
            padding: 40px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 300;
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .main-content {
            padding: 40px;
        }

        .upload-section {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
            border: 2px dashed #dee2e6;
            transition: all 0.3s ease;
        }

        .upload-section:hover {
            border-color: #667eea;
            background: #f0f2ff;
        }

        .upload-area {
            text-align: center;
            padding: 20px;
        }

        .upload-icon {
            font-size: 3rem;
            color: #667eea;
            margin-bottom: 20px;
        }

        .file-input {
            display: none;
        }

        .upload-btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px;
        }

        .upload-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .project-name-section {
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #2c3e50;
        }

        .form-group input {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e1e5e9;
            border-radius: 10px;
            font-size: 1rem;
            transition: border-color 0.3s ease;
        }

        .form-group input:focus {
            outline: none;
            border-color: #667eea;
        }

        .process-btn {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            disabled: opacity 0.6;
        }

        .process-btn:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(40, 167, 69, 0.3);
        }

        .process-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .file-info {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            display: none;
        }

        .results-section {
            margin-top: 30px;
            display: none;
        }

        .results-header {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
            padding: 20px;
            border-radius: 10px 10px 0 0;
        }

        .results-content {
            background: white;
            border: 2px solid #28a745;
            border-top: none;
            border-radius: 0 0 10px 10px;
            padding: 20px;
        }

        .download-btn {
            background: linear-gradient(135deg, #fd7e14, #e83e8c);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-right: 10px;
        }

        .download-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 16px rgba(253, 126, 20, 0.3);
        }

        .wbs-preview {
            max-height: 400px;
            overflow-y: auto;
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            line-height: 1.4;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .error-message {
            background: #f8d7da;
            color: #721c24;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            display: none;
        }

        .success-message {
            background: #d4edda;
            color: #155724;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            display: none;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .stat-card {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }

        .stat-value {
            font-size: 2rem;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 0.9rem;
            opacity: 0.9;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>WBS Generator</h1>
            <p>Upload your equipment list and generate a comprehensive Work Breakdown Structure</p>
        </div>

        <div class="main-content">
            <!-- File Upload Section -->
            <div class="upload-section">
                <div class="upload-area">
                    <div class="upload-icon">📁</div>
                    <h3>Upload Equipment List</h3>
                    <p>Support for CSV, Excel (.xlsx), and JSON files</p>
                    <input type="file" id="fileInput" class="file-input" accept=".csv,.xlsx,.json" />
                    <button class="upload-btn" onclick="document.getElementById('fileInput').click()">
                        Choose File
                    </button>
                    <div class="file-info" id="fileInfo"></div>
                </div>
            </div>

            <!-- Project Name Section -->
            <div class="project-name-section">
                <div class="form-group">
                    <label for="projectName">Project Name:</label>
                    <input type="text" id="projectName" placeholder="Enter project name" value="Sample Commissioning Project" />
                </div>
            </div>

            <!-- Process Button -->
            <div style="text-align: center; margin-bottom: 20px;">
                <button class="process-btn" id="processBtn" onclick="processFile()" disabled>
                    Generate WBS Structure
                </button>
            </div>

            <!-- Loading Indicator -->
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Processing equipment list and generating WBS structure...</p>
            </div>

            <!-- Error Message -->
            <div class="error-message" id="errorMessage"></div>

            <!-- Success Message -->
            <div class="success-message" id="successMessage"></div>

            <!-- Results Section -->
            <div class="results-section" id="resultsSection">
                <div class="results-header">
                    <h3>WBS Structure Generated Successfully</h3>
                </div>
                <div class="results-content">
                    <div class="stats" id="stats"></div>
                    
                    <div style="margin-top: 20px;">
                        <button class="download-btn" onclick="downloadCSV()">
                            📥 Download CSV
                        </button>
                        <button class="download-btn" onclick="downloadJSON()">
                            📋 Download JSON
                        </button>
                    </div>

                    <div class="wbs-preview" id="wbsPreview"></div>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/papaparse/5.4.1/papaparse.min.js"></script>

    <script>
        // Global variables
        let currentFile = null;
        let wbsData = null;
        let generator = null;

        // WBS Generator Class (converted from Python)
        class WBSGenerator {
            constructor() {
                this.wbsCounter = 1;
                this.wbsStructure = [];
                
                this.equipmentTypes = {
                    '+UH': 'Protection Panels',
                    '+WC': 'LV Switchboards', 
                    '+WA': 'HV Switchboards',
                    '-F': 'Feeder Protection',
                    '-KF': 'Control Devices',
                    '-Y': 'Network Devices',
                    'CB': 'Circuit Breakers',
                    'CT': 'Current Transformers',
                    'VT': 'Voltage Transformers',
                    '+Z01-X01': 'Building Services',
                    // Additional patterns for your equipment
                    'T': 'Transformers',
                    '+UC': 'Control Systems',
                    '+CA': 'Capacitor Banks',
                    '+HN': 'Auxiliary Systems',
                    'D': 'Protection Devices',
                    'E': 'Protection Devices'
                };
                
                this.equipmentPatterns = {
                    '\\+UH\\d+': 'Protection Panels',
                    '\\+WC\\d+': 'LV Switchboards',
                    '\\+WA\\d+': 'HV Switchboards',
                    '.*-F\\d+': 'Feeder Protection',
                    '.*-KF\\d+': 'Control Devices',
                    '.*-Y\\d+': 'Network Devices',
                    '.*CB\\d+': 'Circuit Breakers',
                    '.*CT\\d+': 'Current Transformers',
                    '.*VT\\d+': 'Voltage Transformers',
                    '\\+Z\\d+-X\\d+': 'Building Services',
                    // Additional patterns for your specific equipment
                    '^T\\d+': 'Transformers',
                    '\\+UC\\d+': 'Control Systems',
                    '\\+CA\\d+': 'Capacitor Banks',
                    '\\+HN\\d+': 'Auxiliary Systems',
                    '^D\\d+': 'Protection Devices',
                    '^E\\d+': 'Protection Devices',
                    'D\\d+\\s*/\\s*E\\d+': 'Protection Devices'
                };
            }

            parseEquipmentList(equipmentData) {
                const equipmentGroups = {};
                const orphanEquipment = [];
                
                // Debug: Log the first few items to see column names and mapped values
                if (equipmentData.length > 0) {
                    console.log('Available columns in your data:', Object.keys(equipmentData[0]));
                    console.log('First equipment item (raw):', equipmentData[0]);
                    
                    // Show how the mapping worked for first item
                    const firstItem = equipmentData[0];
                    const mappedParent = (
                        firstItem.parent || 
                        firstItem.Parent || 
                        firstItem['parent equipment number'] ||
                        firstItem['Parent Equipment Number'] ||
                        firstItem['parent_equipment_number'] ||
                        firstItem['Parent_Equipment_Number'] ||
                        firstItem['parent equipment'] ||
                        firstItem['Parent Equipment'] ||
                        firstItem['parent_equipment'] ||
                        firstItem['Parent_Equipment'] ||
                        firstItem['parent number'] ||
                        firstItem['Parent Number'] ||
                        firstItem['parent_number'] ||
                        firstItem['Parent_Number'] ||
                        firstItem['parent eq'] ||
                        firstItem['Parent Eq'] ||
                        firstItem['parent_eq'] ||
                        firstItem['Parent_Eq'] ||
                        firstItem['parent code'] ||
                        firstItem['Parent Code'] ||
                        firstItem['parent_code'] ||
                        firstItem['Parent_Code'] ||
                        ''
                    ).trim();
                    
                    const mappedEquipment = (
                        firstItem.equipment || 
                        firstItem.Equipment ||
                        firstItem['equipment number'] || 
                        firstItem['Equipment Number'] ||
                        firstItem['equipment_number'] || 
                        firstItem['Equipment_Number'] ||
                        firstItem['equipment code'] ||
                        firstItem['Equipment Code'] ||
                        firstItem['equipment_code'] ||
                        firstItem['Equipment_Code'] ||
                        firstItem['eq number'] ||
                        firstItem['Eq Number'] ||
                        firstItem['eq_number'] ||
                        firstItem['Eq_Number'] ||
                        firstItem['eq code'] ||
                        firstItem['Eq Code'] ||
                        firstItem['eq_code'] ||
                        firstItem['Eq_Code'] ||
                        firstItem.tag ||
                        firstItem.Tag ||
                        firstItem.TAG ||
                        firstItem.number ||
                        firstItem.Number ||
                        firstItem.code ||
                        firstItem.Code ||
                        ''
                    ).trim();
                    
                    console.log('Mapped values for first item:');
                    console.log('  - Parent:', mappedParent || '(empty)');
                    console.log('  - Equipment:', mappedEquipment || '(empty)');
                    console.log('  - PLU/Description:', (
                        firstItem.plu_name || 
                        firstItem['PLU Name'] ||
                        firstItem['plu name'] ||
                        firstItem['Plu Name'] ||
                        firstItem.plu || 
                        firstItem.PLU || 
                        firstItem.Plu ||
                        firstItem.description || 
                        firstItem.Description ||
                        firstItem.DESCRIPTION ||
                        firstItem.desc ||
                        firstItem.Desc ||
                        firstItem.DESC ||
                        firstItem.name ||
                        firstItem.Name ||
                        firstItem.NAME ||
                        ''
                    ).trim() || '(empty)');
                }
                
                for (const item of equipmentData) {
                    // Handle multiple possible column name variations with comprehensive mapping
                    const parent = (
                        item.parent || 
                        item.Parent || 
                        item['parent equipment number'] ||
                        item['Parent Equipment Number'] ||
                        item['parent_equipment_number'] ||
                        item['Parent_Equipment_Number'] ||
                        item['parent equipment'] ||
                        item['Parent Equipment'] ||
                        item['parent_equipment'] ||
                        item['Parent_Equipment'] ||
                        item['parent number'] ||
                        item['Parent Number'] ||
                        item['parent_number'] ||
                        item['Parent_Number'] ||
                        item['parent eq'] ||
                        item['Parent Eq'] ||
                        item['parent_eq'] ||
                        item['Parent_Eq'] ||
                        item['parent code'] ||
                        item['Parent Code'] ||
                        item['parent_code'] ||
                        item['Parent_Code'] ||
                        ''
                    ).trim();
                    
                    const equipment = (
                        item.equipment || 
                        item.Equipment ||
                        item['equipment number'] || 
                        item['Equipment Number'] ||
                        item['equipment_number'] || 
                        item['Equipment_Number'] ||
                        item['equipment code'] ||
                        item['Equipment Code'] ||
                        item['equipment_code'] ||
                        item['Equipment_Code'] ||
                        item['eq number'] ||
                        item['Eq Number'] ||
                        item['eq_number'] ||
                        item['Eq_Number'] ||
                        item['eq code'] ||
                        item['Eq Code'] ||
                        item['eq_code'] ||
                        item['Eq_Code'] ||
                        item.tag ||
                        item.Tag ||
                        item.TAG ||
                        item.number ||
                        item.Number ||
                        item.code ||
                        item.Code ||
                        ''
                    ).trim();
                    
                    const pluName = (
                        item.plu_name || 
                        item['PLU Name'] ||
                        item['plu name'] ||
                        item['Plu Name'] ||
                        item.plu || 
                        item.PLU || 
                        item.Plu ||
                        item.description || 
                        item.Description ||
                        item.DESCRIPTION ||
                        item.desc ||
                        item.Desc ||
                        item.DESC ||
                        item.name ||
                        item.Name ||
                        item.NAME ||
                        item.title ||
                        item.Title ||
                        item.TITLE ||
                        item.label ||
                        item.Label ||
                        item.LABEL ||
                        ''
                    ).trim();
                    
                    if (!equipment) {
                        console.warn('Skipping item with missing equipment:', item);
                        continue;
                    }
                    
                    if (!parent || parent === '-') {
                        const equipmentType = this.identifyEquipmentType(equipment);
                        orphanEquipment.push({
                            equipment: equipment,
                            type: equipmentType,
                            plu_name: pluName,
                            parent: null
                        });
                    } else {
                        if (!equipmentGroups[parent]) {
                            equipmentGroups[parent] = {};
                        }
                        
                        const equipmentType = this.determineChildEquipmentType(parent, equipment);
                        
                        if (!equipmentGroups[parent][equipmentType]) {
                            equipmentGroups[parent][equipmentType] = [];
                        }
                        
                        equipmentGroups[parent][equipmentType].push({
                            equipment: equipment,
                            plu_name: pluName
                        });
                    }
                }
                
                return { equipmentGroups, orphanEquipment };
            }

            determineChildEquipmentType(parent, equipment) {
                if (equipment.startsWith(parent)) {
                    const suffix = equipment.substring(parent.length).replace(/^-/, '');
                    
                    if (suffix.match(/^F\d+/)) return '-F';
                    if (suffix.match(/^KF\d+/)) return '-KF';
                    if (suffix.match(/^Y\d+/)) return '-Y';
                    if (suffix.includes('CB')) return 'CB';
                    if (suffix.includes('CT')) return 'CT';
                    if (suffix.includes('VT')) return 'VT';
                }
                
                return this.identifyEquipmentType(equipment);
            }

            identifyEquipmentType(equipmentCode) {
                equipmentCode = equipmentCode.toUpperCase().trim();
                
                for (const [eqType, category] of Object.entries(this.equipmentTypes)) {
                    if (equipmentCode.startsWith(eqType)) {
                        return eqType;
                    }
                }
                
                for (const [pattern, category] of Object.entries(this.equipmentPatterns)) {
                    if (equipmentCode.match(new RegExp(pattern, 'i'))) {
                        const matchingType = Object.keys(this.equipmentTypes).find(
                            key => this.equipmentTypes[key] === category
                        );
                        return matchingType;
                    }
                }
                
                return null;
            }

            createWbsNode(name, parentId, shortName = null) {
                if (shortName === null) {
                    const siblings = this.wbsStructure.filter(node => node['Parent WBS'] === parentId);
                    shortName = String(siblings.length + 1);
                }
                
                if (name.length > 100) {
                    console.warn(`WBS name '${name}' exceeds 100 characters`);
                }
                
                const node = {
                    'WBS ID': this.wbsCounter,
                    'WBS Short Name': shortName.substring(0, 20),
                    'Parent WBS': parentId,
                    'WBS': name
                };
                
                this.wbsStructure.push(node);
                const currentId = this.wbsCounter;
                this.wbsCounter++;
                return currentId;
            }

            buildStandardStructure(projectName = "Project Name") {
                // Reset structure
                this.wbsCounter = 1;
                this.wbsStructure = [];
                
                // Root project
                const projectId = this.createWbsNode(projectName, null, "ROOT");
                
                // Main phases
                const fatId = this.createWbsNode("FAT", projectId, "FAT");
                const satId = this.createWbsNode("SAT", projectId, "SAT");
                const energisationId = this.createWbsNode("Energisation", projectId, "ENERG");
                const prerequisitesId = this.createWbsNode("Pre-Requisites", projectId, "PREREQ");
                const milestonesId = this.createWbsNode("Milestones", projectId, "MILES");
                
                // FAT structure
                const fatPanelsId = this.createWbsNode("Panels", fatId, "PANELS");
                const fatPrepId = this.createWbsNode("Preparations and set-up", fatId, "PREP");
                const fatBuildingId = this.createWbsNode("Building Services", fatId, "BLDG");
                const fatWaId = this.createWbsNode("+WA", fatId, "WA");
                const fatInterfaceId = this.createWbsNode("Interface Testing", fatId, "INTFC");
                const fatAncillaryId = this.createWbsNode("Ancillary Systems", fatId, "ANCIL");
                
                // SAT structure
                const satPanelsId = this.createWbsNode("Panels", satId, "PANELS");
                const satPrepId = this.createWbsNode("Preparations and set-up", satId, "PREP");
                const satBuildingId = this.createWbsNode("Building Services", satId, "BLDG");
                const satWaId = this.createWbsNode("+WA", satId, "WA");
                const satInterfaceId = this.createWbsNode("Interface Testing", satId, "INTFC");
                const satAncillaryId = this.createWbsNode("Ancillary Systems", satId, "ANCIL");
                
                // Interface Testing phases
                this.createWbsNode("Phase 1", fatInterfaceId, "PH1");
                this.createWbsNode("Phase 2", fatInterfaceId, "PH2");
                this.createWbsNode("Phase 1", satInterfaceId, "PH1");
                this.createWbsNode("Phase 2", satInterfaceId, "PH2");
                
                // Energisation structure
                const systemId = this.createWbsNode("System", energisationId, "SYS");
                this.createWbsNode("Pre Energisation", systemId, "PRE");
                this.createWbsNode("Energisation", systemId, "ENERG");
                this.createWbsNode("Post Energisation", systemId, "POST");
                
                // Pre-requisites
                this.createWbsNode("Phase 1", prerequisitesId, "PH1");
                this.createWbsNode("Phase 2", prerequisitesId, "PH2");
                
                // Store key sections
                this.keySections = {
                    'fat_panels': fatPanelsId,
                    'sat_panels': satPanelsId,
                    'fat_wa': fatWaId,
                    'sat_wa': satWaId,
                    'fat_building': fatBuildingId,
                    'sat_building': satBuildingId,
                    'fat_interface': fatInterfaceId,
                    'sat_interface': satInterfaceId,
                    'fat_ancillary': fatAncillaryId,
                    'sat_ancillary': satAncillaryId
                };
                
                return projectId;
            }

            addEquipmentToStructure(equipmentGroups, orphanEquipment) {
                // Handle equipment with parents
                for (const [parentCode, equipmentByType] of Object.entries(equipmentGroups)) {
                    const parentType = this.identifyEquipmentType(parentCode);
                    
                    if (parentType === '+UH') {
                        this.addProtectionPanelEquipment(parentCode, equipmentByType, equipmentGroups);
                    } else if (parentType === '+WC') {
                        this.addLvSwitchboardEquipment(parentCode, equipmentByType, equipmentGroups);
                    } else if (parentType === '+WA') {
                        this.addHvSwitchboardEquipment(parentCode, equipmentByType, equipmentGroups);
                    } else if (['-F', '-KF', '-Y', 'CB', 'CT', 'VT'].includes(parentType)) {
                        this.addChildEquipment(parentCode, equipmentByType, equipmentGroups);
                    } else {
                        this.addToAncillary(parentCode, equipmentByType);
                    }
                }
                
                // Handle orphan equipment
                this.addOrphanEquipment(orphanEquipment);
            }

            addProtectionPanelEquipment(parentCode, equipmentByType, allEquipmentGroups) {
                /**Add protection panel equipment (+UH) to FAT and SAT sections*/
                for (const sectionKey of ['fat_protection_panels', 'sat_protection_panels']) {
                    const parentId = this.keySections[sectionKey];
                    const equipmentParentId = this.createWbsNode(parentCode, parentId);
                    
                    // Protection panels: Feeder Protection and Network Devices subsections
                    const feederProtId = this.createWbsNode("Feeder Protection", equipmentParentId, "FEED_PROT");
                    const networkDevId = this.createWbsNode("Network Devices", equipmentParentId, "NET_DEV");
                    const controlDevId = this.createWbsNode("Control Devices", equipmentParentId, "CTRL_DEV");
                    
                    // Add equipment to appropriate subsections
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        let targetParent, subsectionName;
                        
                        if (eqType === '-F') {
                            targetParent = feederProtId;
                            subsectionName = "Feeder Protection Relays";
                        } else if (eqType === '-Y') {
                            targetParent = networkDevId;
                            subsectionName = "Network Devices";
                        } else if (eqType === '-KF') {
                            targetParent = controlDevId;
                            subsectionName = "Control Devices";
                        } else if (eqType === null) {
                            // Unrecognized equipment goes to Ancillary Systems
                            const ancillaryParent = this.keySections[sectionKey.replace('protection_panels', 'ancillary')];
                            for (const eqItem of equipmentList) {
                                this.createWbsNode(eqItem.equipment, ancillaryParent);
                            }
                            continue;
                        } else {
                            continue;
                        }
                        
                        // Create subsection for equipment type
                        const subsectionId = this.createWbsNode(subsectionName, targetParent);
                        
                        for (const eqItem of equipmentList) {
                            const equipmentId = this.createWbsNode(eqItem.equipment, subsectionId);
                            
                            // Check if this equipment has its own children
                            this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                        }
                    }
                }
            }

            addLvSwitchboardEquipment(parentCode, equipmentByType, allEquipmentGroups) {
                /**Add LV switchboard equipment (+WC) to FAT and SAT sections*/
                for (const sectionKey of ['fat_lv_switchboards', 'sat_lv_switchboards']) {
                    const parentId = this.keySections[sectionKey];
                    const equipmentParentId = this.createWbsNode(parentCode, parentId);
                    
                    // LV Switchboards: Protection Devices, Metering Devices, Current Transformers
                    const protId = this.createWbsNode("Protection Devices", equipmentParentId, "PROT");
                    const meterId = this.createWbsNode("Metering Devices", equipmentParentId, "METER");
                    const ctId = this.createWbsNode("Current Transformers", equipmentParentId, "CT");
                    
                    // Add equipment to appropriate subsections
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        let targetParent;
                        
                        if (['PROT', 'PROTECTION'].includes(eqType)) {
                            targetParent = protId;
                        } else if (['METER', 'METERING'].includes(eqType)) {
                            targetParent = meterId;
                        } else if (eqType === 'CT') {
                            targetParent = ctId;
                        } else if (eqType === null) {
                            // Unrecognized equipment goes to Ancillary Systems
                            const ancillaryParent = this.keySections[sectionKey.replace('lv_switchboards', 'ancillary')];
                            for (const eqItem of equipmentList) {
                                this.createWbsNode(eqItem.equipment, ancillaryParent);
                            }
                            continue;
                        } else {
                            // For LV switchboards, categorize based on equipment name patterns
                            targetParent = this.determineWcCategory(eqType, protId, meterId, ctId);
                        }
                        
                        for (const eqItem of equipmentList) {
                            const equipmentId = this.createWbsNode(eqItem.equipment, targetParent);
                            
                            // Check if this equipment has its own children
                            this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                        }
                    }
                }
            }

            addHvSwitchboardEquipment(parentCode, equipmentByType, allEquipmentGroups) {
                /**Add HV switchboard equipment (+WA) to FAT and SAT sections*/
                for (const sectionKey of ['fat_hv_switchboards', 'sat_hv_switchboards']) {
                    const parentId = this.keySections[sectionKey];
                    const tiersId = this.createWbsNode("Tiers", parentId, "TIERS");
                    const equipmentParentId = this.createWbsNode(parentCode, tiersId);
                    
                    // Create standard subsections (CB's, Current Transformers, Voltage Transformers)
                    const cbId = this.createWbsNode("Circuit Breakers", equipmentParentId, "CB");
                    const ctId = this.createWbsNode("Current Transformers", equipmentParentId, "CT");
                    const vtId = this.createWbsNode("Voltage Transformers", equipmentParentId, "VT");
                    
                    // Add equipment to appropriate subsections
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        let targetParent;
                        
                        if (eqType === 'CB') {
                            targetParent = cbId;
                        } else if (eqType === 'CT') {
                            targetParent = ctId;
                        } else if (eqType === 'VT') {
                            targetParent = vtId;
                        } else if (eqType === null) {
                            // Unrecognized equipment goes to Ancillary Systems
                            const ancillaryParent = this.keySections[sectionKey.replace('hv_switchboards', 'ancillary')];
                            for (const eqItem of equipmentList) {
                                this.createWbsNode(eqItem.equipment, ancillaryParent);
                            }
                            continue;
                        } else {
                            continue;
                        }
                            
                        for (const eqItem of equipmentList) {
                            const equipmentId = this.createWbsNode(eqItem.equipment, targetParent);
                            // Check if this equipment has its own children
                            this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                        }
                    }
                }
            }
                const parentType = this.identifyEquipmentType(parentCode);
                
                for (const sectionKey of ['fat_panels', 'sat_panels']) {
                    const parentId = this.keySections[sectionKey];
                    const equipmentParentId = this.createWbsNode(parentCode, parentId);
                    
                    if (parentType === '+UH') {
                        const initId = this.createWbsNode("Initialization", equipmentParentId, "INIT");
                        const devicesId = this.createWbsNode("Devices", equipmentParentId, "DEV");
                        
                        for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                            let targetParent, subsectionName;
                            
                            if (eqType === '-F') {
                                targetParent = initId;
                                subsectionName = "-F";
                            } else if (['-KF', '-Y'].includes(eqType)) {
                                targetParent = devicesId;
                                subsectionName = eqType;
                            } else if (eqType === null) {
                                const ancillaryParent = this.keySections[sectionKey.replace('panels', 'ancillary')];
                                for (const eqItem of equipmentList) {
                                    this.createWbsNode(eqItem.equipment, ancillaryParent);
                                }
                                continue;
                            } else {
                                continue;
                            }
                            
                            const subsectionId = this.createWbsNode(subsectionName, targetParent);
                            
                            for (const eqItem of equipmentList) {
                                const equipmentId = this.createWbsNode(eqItem.equipment, subsectionId);
                                this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                            }
                        }
                    } else if (parentType === '+WC') {
                        const protId = this.createWbsNode("Protection Devices", equipmentParentId, "PROT");
                        const meterId = this.createWbsNode("Metering Devices", equipmentParentId, "METER");
                        const ctId = this.createWbsNode("Current Transformers", equipmentParentId, "CT");
                        
                        for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                            let targetParent;
                            
                            if (['PROT', 'PROTECTION'].includes(eqType)) {
                                targetParent = protId;
                            } else if (['METER', 'METERING'].includes(eqType)) {
                                targetParent = meterId;
                            } else if (eqType === 'CT') {
                                targetParent = ctId;
                            } else if (eqType === null) {
                                const ancillaryParent = this.keySections[sectionKey.replace('panels', 'ancillary')];
                                for (const eqItem of equipmentList) {
                                    this.createWbsNode(eqItem.equipment, ancillaryParent);
                                }
                                continue;
                            } else {
                                targetParent = this.determineWcCategory(eqType, protId, meterId, ctId);
                            }
                            
                            for (const eqItem of equipmentList) {
                                const equipmentId = this.createWbsNode(eqItem.equipment, targetParent);
                                this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                            }
                        }
                    }
                }
            }

            determineWcCategory(eqType, protId, meterId, ctId) {
                if (eqType && eqType.toUpperCase().includes('CT')) {
                    return ctId;
                } else if (eqType && ['PROT', 'RELAY', 'SWITCH'].some(word => eqType.toUpperCase().includes(word))) {
                    return protId;
                } else {
                    return meterId;
                }
            }

            addNestedChildren(parentEquipment, parentWbsId, allEquipmentGroups) {
                if (allEquipmentGroups[parentEquipment]) {
                    const childEquipmentByType = allEquipmentGroups[parentEquipment];
                    
                    for (const [eqType, equipmentList] of Object.entries(childEquipmentByType)) {
                        if (eqType === 'CB') {
                            const cbSectionId = this.createWbsNode("Circuit Breakers", parentWbsId, "CB");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, cbSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else if (eqType === 'CT') {
                            const ctSectionId = this.createWbsNode("Current Transformers", parentWbsId, "CT");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, ctSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else if (eqType === 'VT') {
                            const vtSectionId = this.createWbsNode("Voltage Transformers", parentWbsId, "VT");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, vtSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else {
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, parentWbsId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        }
                    }
                }
            }

            addChildEquipment(parentCode, equipmentByType, allEquipmentGroups) {
                const parentNodes = this.wbsStructure.filter(node => node.WBS.includes(parentCode));
                
                for (const parentNode of parentNodes) {
                    const parentWbsId = parentNode['WBS ID'];
                    
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        if (eqType === 'CB') {
                            const cbSectionId = this.createWbsNode("Circuit Breakers", parentWbsId, "CB");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, cbSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else if (eqType === 'CT') {
                            const ctSectionId = this.createWbsNode("Current Transformers", parentWbsId, "CT");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, ctSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else if (eqType === 'VT') {
                            const vtSectionId = this.createWbsNode("Voltage Transformers", parentWbsId, "VT");
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, vtSectionId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        } else {
                            for (const eqItem of equipmentList) {
                                const childEquipmentId = this.createWbsNode(eqItem.equipment, parentWbsId);
                                this.addNestedChildren(eqItem.equipment, childEquipmentId, allEquipmentGroups);
                            }
                        }
                    }
                }
            }

            addTierEquipment(parentCode, equipmentByType, allEquipmentGroups) {
                for (const sectionKey of ['fat_wa', 'sat_wa']) {
                    const parentId = this.keySections[sectionKey];
                    const tiersId = this.createWbsNode("Tiers", parentId, "TIERS");
                    const equipmentParentId = this.createWbsNode(parentCode, tiersId);
                    
                    const cbId = this.createWbsNode("CB's", equipmentParentId, "CB");
                    const ctId = this.createWbsNode("Current Transformers (CT)", equipmentParentId, "CT");
                    const vtId = this.createWbsNode("Voltage Transformers (VT)", equipmentParentId, "VT");
                    
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        let targetParent;
                        
                        if (eqType === 'CB') {
                            targetParent = cbId;
                        } else if (eqType === 'CT') {
                            targetParent = ctId;
                        } else if (eqType === 'VT') {
                            targetParent = vtId;
                        } else if (eqType === null) {
                            const ancillaryParent = this.keySections[sectionKey.replace('wa', 'ancillary')];
                            for (const eqItem of equipmentList) {
                                this.createWbsNode(eqItem.equipment, ancillaryParent);
                            }
                            continue;
                        } else {
                            continue;
                        }
                        
                        for (const eqItem of equipmentList) {
                            const equipmentId = this.createWbsNode(eqItem.equipment, targetParent);
                            this.addNestedChildren(eqItem.equipment, equipmentId, allEquipmentGroups);
                        }
                    }
                }
            }

            addToAncillary(parentCode, equipmentByType) {
                for (const sectionKey of ['fat_ancillary', 'sat_ancillary']) {
                    const parentId = this.keySections[sectionKey];
                    const equipmentParentId = this.createWbsNode(parentCode, parentId);
                    
                    for (const [eqType, equipmentList] of Object.entries(equipmentByType)) {
                        for (const eqItem of equipmentList) {
                            this.createWbsNode(eqItem.equipment, equipmentParentId);
                        }
                    }
                }
            }

            addOrphanEquipment(orphanEquipment) {
                for (const eqItem of orphanEquipment) {
                    const eqType = eqItem.type;
                    const equipmentName = eqItem.equipment;
                    
                    if (['+UH', '+WC'].includes(eqType)) {
                        for (const sectionKey of ['fat_panels', 'sat_panels']) {
                            const parentId = this.keySections[sectionKey];
                            const equipmentId = this.createWbsNode(equipmentName, parentId);
                            
                            this.createWbsNode("Initialization", equipmentId, "INIT");
                            this.createWbsNode("Devices", equipmentId, "DEV");
                        }
                    } else if (eqType === '+Z01-X01' || (eqType && eqType.includes('Building Services'))) {
                        for (const sectionKey of ['fat_building', 'sat_building']) {
                            const parentId = this.keySections[sectionKey];
                            this.createWbsNode(equipmentName, parentId);
                        }
                    } else {
                        for (const sectionKey of ['fat_ancillary', 'sat_ancillary']) {
                            const parentId = this.keySections[sectionKey];
                            this.createWbsNode(equipmentName, parentId);
                        }
                    }
                }
            }

            generateWbs(equipmentData, projectName = "Project Name") {
                if (!equipmentData || equipmentData.length === 0) {
                    console.warn("No equipment data provided");
                    return [];
                }
                
                const { equipmentGroups, orphanEquipment } = this.parseEquipmentList(equipmentData);
                
                this.buildStandardStructure(projectName);
                
                this.addEquipmentToStructure(equipmentGroups, orphanEquipment);
                
                console.log(`Generated WBS with ${this.wbsStructure.length} nodes`);
                return this.wbsStructure;
            }
        }

        // File handling functions
        document.getElementById('fileInput').addEventListener('change', function(event) {
            const file = event.target.files[0];
            if (file) {
                currentFile = file;
                displayFileInfo(file);
                document.getElementById('processBtn').disabled = false;
            }
        });

        function displayFileInfo(file) {
            const fileInfo = document.getElementById('fileInfo');
            fileInfo.innerHTML = `
                <strong>File:</strong> ${file.name}<br>
                <strong>Size:</strong> ${(file.size / 1024).toFixed(2)} KB<br>
                <strong>Type:</strong> ${file.type || 'Unknown'}
            `;
            fileInfo.style.display = 'block';
        }

        function showError(message) {
            const errorDiv = document.getElementById('errorMessage');
            errorDiv.textContent = message;
            errorDiv.style.display = 'block';
            setTimeout(() => {
                errorDiv.style.display = 'none';
            }, 5000);
        }

        function showSuccess(message) {
            const successDiv = document.getElementById('successMessage');
            successDiv.textContent = message;
            successDiv.style.display = 'block';
            setTimeout(() => {
                successDiv.style.display = 'none';
            }, 3000);
        }

        // File processing function
        async function processFile() {
            if (!currentFile) {
                showError('Please select a file first');
                return;
            }

            document.getElementById('loading').style.display = 'block';
            document.getElementById('resultsSection').style.display = 'none';
            document.getElementById('processBtn').disabled = true;

            try {
                let equipmentData = [];
                const projectName = document.getElementById('projectName').value || 'Sample Commissioning Project';

                if (currentFile.name.endsWith('.csv')) {
                    equipmentData = await parseCSV(currentFile);
                } else if (currentFile.name.endsWith('.xlsx')) {
                    equipmentData = await parseExcel(currentFile);
                } else if (currentFile.name.endsWith('.json')) {
                    equipmentData = await parseJSON(currentFile);
                } else {
                    throw new Error('Unsupported file format');
                }

                if (!equipmentData || equipmentData.length === 0) {
                    throw new Error('No valid equipment data found in file');
                }

                console.log('Equipment data sample:', equipmentData.slice(0, 5));
                console.log('Total equipment items:', equipmentData.length);

                // Generate WBS
                generator = new WBSGenerator();
                wbsData = generator.generateWbs(equipmentData, projectName);

                displayResults();
                showSuccess(`WBS structure generated successfully with ${wbsData.length} nodes`);

            } catch (error) {
                console.error('Processing error:', error);
                showError(`Error processing file: ${error.message}`);
            } finally {
                document.getElementById('loading').style.display = 'none';
                document.getElementById('processBtn').disabled = false;
            }
        }

        // File parsing functions
        function parseCSV(file) {
            return new Promise((resolve, reject) => {
                Papa.parse(file, {
                    header: true,
                    dynamicTyping: true,
                    skipEmptyLines: true,
                    transformHeader: function(header) {
                        // Clean and standardize headers
                        return header.trim().toLowerCase().replace(/\s+/g, '_');
                    },
                    complete: function(results) {
                        if (results.errors.length > 0) {
                            console.warn('CSV parsing warnings:', results.errors);
                        }
                        console.log('Parsed CSV data:', results.data.slice(0, 3)); // Log first 3 rows
                        resolve(results.data);
                    },
                    error: function(error) {
                        reject(error);
                    }
                });
            });
        }

        function parseExcel(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const data = new Uint8Array(e.target.result);
                        const workbook = XLSX.read(data, { type: 'array' });
                        const sheetName = workbook.SheetNames[0];
                        const worksheet = workbook.Sheets[sheetName];
                        const jsonData = XLSX.utils.sheet_to_json(worksheet);
                        resolve(jsonData);
                    } catch (error) {
                        reject(error);
                    }
                };
                reader.onerror = reject;
                reader.readAsArrayBuffer(file);
            });
        }

        function parseJSON(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const jsonData = JSON.parse(e.target.result);
                        resolve(Array.isArray(jsonData) ? jsonData : [jsonData]);
                    } catch (error) {
                        reject(error);
                    }
                };
                reader.onerror = reject;
                reader.readAsText(file);
            });
        }

        // Results display function
        function displayResults() {
            const resultsSection = document.getElementById('resultsSection');
            const statsDiv = document.getElementById('stats');
            const previewDiv = document.getElementById('wbsPreview');

            // Calculate statistics
            const totalNodes = wbsData.length;
            const rootNodes = wbsData.filter(node => node['Parent WBS'] === null).length;
            const leafNodes = wbsData.filter(node => 
                !wbsData.some(other => other['Parent WBS'] === node['WBS ID'])
            ).length;

            // Display statistics
            statsDiv.innerHTML = `
                <div class="stat-card">
                    <div class="stat-value">${totalNodes}</div>
                    <div class="stat-label">Total Nodes</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value">${rootNodes}</div>
                    <div class="stat-label">Root Nodes</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value">${leafNodes}</div>
                    <div class="stat-label">Leaf Nodes</div>
                </div>
            `;

            // Display WBS preview
            let previewText = 'WBS Structure Preview:\n\n';
            
            function getLevel(nodeId) {
                const node = wbsData.find(n => n['WBS ID'] === nodeId);
                if (!node || node['Parent WBS'] === null) return 0;
                return 1 + getLevel(node['Parent WBS']);
            }

            for (const node of wbsData.slice(0, 50)) { // Show first 50 nodes
                const level = getLevel(node['WBS ID']);
                const indent = '  '.repeat(level);
                previewText += `${node['WBS ID'].toString().padStart(2)} | ${indent}${node['WBS']}\n`;
            }

            if (wbsData.length > 50) {
                previewText += `\n... and ${wbsData.length - 50} more nodes`;
            }

            previewDiv.textContent = previewText;
            resultsSection.style.display = 'block';
        }

        // Download functions
        function downloadCSV() {
            if (!wbsData) {
                showError('No WBS data to download');
                return;
            }

            const csvContent = convertToCSV(wbsData);
            const blob = new Blob([csvContent], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = 'wbs_structure.csv';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);
            
            showSuccess('CSV file downloaded successfully');
        }

        function downloadJSON() {
            if (!wbsData) {
                showError('No WBS data to download');
                return;
            }

            const jsonContent = JSON.stringify(wbsData, null, 2);
            const blob = new Blob([jsonContent], { type: 'application/json' });
            const url = window.URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = 'wbs_structure.json';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);
            
            showSuccess('JSON file downloaded successfully');
        }

        function convertToCSV(data) {
            if (!data || data.length === 0) return '';
            
            const headers = Object.keys(data[0]);
            const csvRows = [];
            
            // Add headers
            csvRows.push(headers.join(','));
            
            // Add data rows
            for (const row of data) {
                const values = headers.map(header => {
                    const value = row[header];
                    // Handle null/undefined values and escape quotes
                    if (value === null || value === undefined) return '';
                    const stringValue = String(value);
                    // Escape quotes and wrap in quotes if contains comma or quote
                    if (stringValue.includes(',') || stringValue.includes('"') || stringValue.includes('\n')) {
                        return `"${stringValue.replace(/"/g, '""')}"`;
                    }
                    return stringValue;
                });
                csvRows.push(values.join(','));
            }
            
            return csvRows.join('\n');
        }

        // Initialize the application
        console.log('WBS Generator Application Loaded');
    </script>
</body>
</html>
