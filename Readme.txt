CRP 4560/5560 – Census Tract Analysis
=====================================

Project goal
------------
This project joins American Community Survey (ACS) household data to Story County
census tracts, maps the results in ArcGIS Pro, and creates a histogram of the
same data using a custom Python toolbox.

----------------------------------------------------------------------
1. DIRECTORY STRUCTURE
----------------------------------------------------------------------

|-- Code/
|     |-- census_toolbox.pyt
|     |-- Story_TotalHouseholds_join.csv
|     |-- Story_Tracts_2020.geojson
|
|-- Data/
|     |-- CensusData.gdb/
|             |-- Story_Tracts_Final (feature class)
|             |-- story_tracts_joined (output feature class)
|
|-- Output/
|     |-- Households_final.png   (histogram created by tool)
|     |-- Final_Map.png          (exported ArcGIS Pro layout)
|
|-- Screenshots/
|     |-- Map_creation_steps.png
|     |-- Symbology_settings.png
|     |-- Join_successful.png
|
|-- Narrative_Document/
|     |-- CensusTractAnalysis.docx
|
|-- README.txt  (this file)

----------------------------------------------------------------------

Key inputs
----------
1. ACS CSV file
   - Name: Story_TotalHouseholds_join.csv
   - Important fields:
     - NAME20_FIXED : tract name text (e.g. "Census Tract 1.01")
     - NAME20       : numeric tract ID as text (e.g. "1.01")
     - TRACT_TEXT   : duplicate numeric tract ID (unused in final tool)
     - Total_HH     : total households (string)
     - Total_HH_num : total households (numeric)

2. Census tract GeoJSON
   - Name: Story_Tracts_2020.geojson
   - Contains one polygon per census tract.
   - Important fields:
     - NAME20    : numeric tract ID as text (e.g. "1.01")

3. Python toolbox
   - File: census_toolbox.pyt
   - Tool: "Census Tract Analysis"

4. Output locations
   - Feature class: created in a folder or geodatabase (e.g. ramesh_project/output)
   - PNG graph: histogram of Total_HH_num values

How to run the tool in ArcGIS Pro
---------------------------------
1. Add the toolbox
   - Catalog pane → Toolboxes → right-click → Add Toolbox
   - Browse to census_toolbox.pyt and add it.

2. Open the tool
   - In Catalog → Toolboxes → Census Toolbox → double-click "Census Tract Analysis".

3. Fill in parameters
   - ACS CSV table:
       ...\code_2_dec_2\Story_TotalHouseholds_join.csv
   - Tract GeoJSON:
       ...\code_2_dec_2\Story_Tracts_2020.geojson
   - Output Geodatabase or Folder:
       e.g. ...\code_2_dec_2\ramesh_project\output
   - Name for output feature class:
       story_tracts_joined
   - Join field name in CSV (e.g. Geography):
       NAME20_FIXED
   - Join field name in feature class (e.g. NAMELSAD):
       NAME20_JOIN
   - Numeric data field to map & graph (from CSV):
       Total_HH_num
   - Display option for feature class:
       Graduated colors
   - Output graph PNG:
       e.g. ...\code_2_dec_2\ramesh_project\output\Households_final.png

4. Run the tool
   - Click "Run".
   - The tool:
       - Converts the GeoJSON polygon layer to a feature class.
       - Creates a clean text field NAME20_JOIN in the feature class.
       - Joins the CSV to the tracts using NAME20_FIXED ↔ NAME20_JOIN.
       - Checks that the join succeeded.
       - Reads Total_HH_num values and builds a histogram PNG.

5. Symbolize the map
   - Add the output feature class story_tracts_joined to the map if needed.
   - In the Symbology pane:
       - Primary symbology: Graduated Colors
       - Field: Total_HH_num (from the joined table)
   - Adjust classes / color ramp as desired.
   - Switch to Layout view and export a final map as JPEG (e.g. Story_Households_Map.jpg).

Outputs
-------
- story_tracts_joined.*  (joined polygon layer of Story County tracts)
- Households_final.png   (histogram of Total_HH_num)
- Story_Households_Map.jpg  (exported map for the report)

Notes
-----
- The tool will stop with an error if the join produces 0 features or if
  the chosen numeric field does not contain any numeric data.
- All processing is done in ArcGIS Pro using ArcPy and matplotlib.
