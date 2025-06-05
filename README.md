# WBS Generator for Commissioning Projects

A specialized Work Breakdown Structure (WBS) generator designed for electrical commissioning projects that automatically creates comprehensive project structures by parsing equipment lists and mapping them to standardized commissioning phases.

## Overview

The WBS Generator streamlines the creation of work breakdown structures for electrical commissioning projects by:
- Processing equipment lists from CSV, Excel (.xlsx), or JSON files
- Automatically categorizing equipment by type and parent-child relationships
- Generating standardized FAT (Factory Acceptance Test) and SAT (Site Acceptance Test) structures
- Creating properly nested WBS hierarchies with unique identifiers
- Exporting P6-compatible CSV files for direct import into Primavera P6

## How It Works

### 1. **Input: Equipment List**
The generator processes equipment lists with the following required columns:
- `Parent Equipment Number` - References parent equipment (use "-" for root-level equipment)
- `Equipment Number` - Unique equipment identifier/tag
- `PLU` - Product Line Unit category
- `Description` - Equipment description

**Example Equipment List:**
```
Parent Equipment Number | Equipment Number | PLU              | Description
-                      | T01              | Transformers     | 33/11kV 16MVA Main Power Transformer
-                      | +WA10            | 33kV Equipment   | 33kV Main Switchboard
+WA10                  | +WA10-A01        | 33kV Equipment   | 630A Incomer & Bus VT Tier
+WA10-A01              | CB10             | Circuit Breakers | 630A Incomer CB
```

### 2. **Processing: Equipment Categorization**
The generator automatically recognizes and categorizes equipment based on naming conventions:

| Equipment Code | Category | WBS Placement |
|---|---|---|
| `+UH` | Protection Panels | FAT/SAT - Protection Panels |
| `+WC` | LV Switchboards | FAT/SAT - LV Switchboards |
| `+WA` | HV Switchboards | FAT/SAT - HV Switchboards |
| `-F` | Feeder Protection | Under Protection Panels |
| `-KF` | Control Devices | Under Protection Panels |
| `-Y` | Network Devices | Under Protection Panels |
| `CB` | Circuit Breakers | Under parent switchboard |
| `CT` | Current Transformers | Under parent switchboard |
| `VT` | Voltage Transformers | Under parent switchboard |
| `T##` | Transformers | FAT/SAT - Transformers |
| `+Z##-X##` | Building Services | FAT/SAT - Building Services |
| `+UC` | Control Systems | FAT/SAT - Ancillary Systems |

### 3. **Output: Standardized WBS Structure**

The generator creates a comprehensive WBS structure following commissioning best practices:

```
1 | Sample Commissioning Project
2 |   FAT
7 |     FAT - Protection Panels
275 |       +UH01
276 |         Feeder Protection
279 |           -F10
280 |           -F12
277 |         Network Devices
278 |         Control Devices
8 |     FAT - LV Switchboards
9 |     FAT - HV Switchboards
35 |       Tiers
36 |         +WA10
45 |           Circuit Breakers
47 |             CB10
46 |           Current Transformers
48 |             CT10
47 |           Voltage Transformers
49 |             VT01
10 |     FAT - Preparations and set-up
11 |     FAT - Building Services
12 |     FAT - Interface Testing
13 |     FAT - Ancillary Systems
14 |     FAT - Transformers
15 |     FAT - Protection Systems
3 |   SAT
16 |     SAT - Protection Panels
17 |     SAT - LV Switchboards
18 |     SAT - HV Switchboards
19 |     SAT - Preparations and set-up
20 |     SAT - Building Services
21 |     SAT - Interface Testing
22 |     SAT - Ancillary Systems
23 |     SAT - Transformers
24 |     SAT - Protection Systems
4 |   Energisation
25 |     System
26 |       Pre Energisation
27 |       Energisation
28 |       Post Energisation
5 |   Pre-Requisites
29 |     Phase 1
30 |     Phase 2
6 |   Milestones
```

## Features

### 🔧 **Equipment Processing**
- **Multi-format Support**: Import CSV, Excel (.xlsx), and JSON files
- **Intelligent Parsing**: Automatically detects equipment hierarchies and relationships
- **Duplicate Prevention**: Filters out problematic relationships (self-references, circular dependencies)
- **Flexible Column Mapping**: Handles various column naming conventions

### 📊 **Standardized Structure**
- **Consistent Framework**: Every project gets the same base structure
- **Dual Testing Phases**: Separate FAT and SAT structures for each equipment category
- **Commissioning Phases**: Includes Energisation, Pre-Requisites, and Milestones
- **Nested Hierarchies**: Proper parent-child relationships maintained throughout

### 📤 **P6-Ready Export**
- **CSV Format**: Direct import into Primavera P6
- **Unique WBS Codes**: Sequential numbering for each WBS element
- **Parent References**: Proper hierarchical structure maintained
- **JSON Export**: Alternative format for other project management tools

## Usage

### Step 1: Prepare Equipment List
Create a spreadsheet with your equipment data:
- Use "-" for root-level equipment (no parent)
- Ensure equipment numbers follow standard naming conventions
- Include complete parent-child relationships

### Step 2: Upload and Process
1. Open the WBS Generator application
2. Click "Choose File" and select your equipment list
3. Enter your project name
4. Click "Generate WBS Structure"

### Step 3: Review and Export
1. Review the generated WBS statistics and preview
2. Download the CSV file for P6 import
3. Optionally download JSON format for other tools

## Output Format

The generator produces a CSV file with three columns optimized for P6 import:

| Column | Description | Example |
|---|---|---|
| `wbs_code` | Unique WBS identifier | 275 |
| `parent_wbs_code` | Parent WBS reference (null for root) | 7 |
| `wbs_name` | WBS element name | +UH01 |

**Sample Output:**
```csv
wbs_code,parent_wbs_code,wbs_name
1,,Sample Commissioning Project
2,1,FAT
7,2,FAT - Protection Panels
275,7,+UH01
279,276,-F10
```

## Technical Specifications

- **Input Formats**: CSV, Excel (.xlsx), JSON
- **Output Formats**: CSV (P6-compatible), JSON
- **Browser Support**: Modern browsers with JavaScript enabled
- **File Size Limit**: Handles equipment lists up to 1000+ items
- **Processing Time**: Typically under 5 seconds for standard projects

## Equipment Naming Conventions

The generator works best when equipment follows standard electrical naming conventions:

### Protection Panels (`+UH##`)
- Main panel: `+UH01`, `+UH02`, etc.
- Child devices: `+UH01-F10` (Feeder Protection), `+UH01-Y10` (Network Device)

### Switchboards (`+WA##` / `+WC##`)
- Main switchboard: `+WA10` (HV), `+WC10` (LV)
- Tiers/Panels: `+WA10-A01`, `+WA10-A02`
- Equipment: `CB10`, `CT10`, `VT01`

### Transformers (`T##`)
- Power transformers: `T01`, `T02`
- Auxiliary transformers: `T10`, `T20`

### Building Services (`+Z##-X##`)
- HVAC units: `+Z01-X01`, `+Z01-X02`
- Fire systems: `FM01`

## Benefits

### For Project Managers
- **Consistency**: Every project follows the same WBS structure
- **Speed**: Generate comprehensive WBS in seconds vs. hours manually
- **Accuracy**: Eliminates manual errors in WBS creation
- **P6 Integration**: Direct import into Primavera P6

### For Commissioning Teams  
- **Complete Coverage**: No equipment missed in the WBS
- **Proper Categorization**: Equipment automatically sorted into correct testing phases
- **Hierarchical Organization**: Clear parent-child relationships maintained

### For Planning Teams
- **Resource Planning**: Clear breakdown for resource allocation
- **Schedule Integration**: WBS ready for activity assignment
- **Progress Tracking**: Hierarchical structure supports roll-up reporting

## Troubleshooting

### Common Issues

**Equipment Not Appearing in Expected Location**
- Check equipment naming conventions match expected patterns
- Verify parent-child relationships are correctly defined
- Review the equipment type classification

**Missing Parent-Child Relationships** 
- Ensure parent equipment numbers exactly match equipment numbers
- Use "-" for root-level equipment only
- Avoid circular references (A->B->A)

**File Upload Errors**
- Verify file format is CSV, Excel (.xlsx), or JSON
- Check that required columns are present
- Ensure file is not corrupted

### Support
For technical issues or feature requests, refer to the application's error messages and console logs for detailed debugging information.

---

*Generated by WBS Generator v2.3 - Optimized for electrical commissioning projects*
