# Description of workflow to create network visualization of tools used together
- taking on [issue #11] (https://github.com/bmkramer/101innovations-survey-data/issues/11)
- using [Gephi] (https://gephi.org/) (version 0.9.1) as visualization tool
- for preset answer options only

## Source files
- co-occurrence matrix (created with [frequencies_of_bivariate_tool_use.R] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/frequencies_of_bivariate_tool_use.R)): [survey_presets_frequencies.csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies.csv)
- tool variables coupled with GEO and TMIE classification (to be used as modularities in network viz): [coupling_variables_toolID_TMIE_GEO.csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/coupling_variables_toolID_TMIE_GEO.csv)

## Step 1: Convert co-occurrence matrix for import in Gephi
Convert co-occurrence matrix for import in Gephi as described here: https://github.com/gephi/gephi/issues/1143.

This will require some permutations in the original co-occurence matrix ([survey_presets_frequencies.csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies.csv))

It would be great to have this step coded in R (but I'll probably do it in Calc for now):

- Replace numbers in first column with column headers transposed (as in last column)
- Remove last column
- Field separator: **semicolon** 
- NB Column headers should not contain spaces

This resulting csv can be imported in Gephi and will be automatically processed into a node and edge table.

Resulting file: [survey_presets_frequencies_edgematrix.csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies_edgematrix.csv)

## Step 2: Import converted co-occurrence matrix into Gephi

- In Gephi, use File > Open to import converted co-occurrence matrix
- In the dialog screen that appears, choose 'undirected' as graph type and click OK
- The matrix will be imported, and the network will be shown in random layout. 

To assign colors, node size etc, information on node attributes will have to be added. In the next step, we will export the node and edge tables from Gephi and enhance the node table with information on attributes.

## Step 3: Export node and edge tables from Gephi
- In the tab Data Laboratory, select 'Nodes' in the menu bar and choose the option 'Export table'
- Choose the following settings:
  - Separator: Comma
  - Charset: UTF-8
  - Columns: Id, Label
- Click OK to start export; save the resulting csv file

- Next, select 'Edges' in the menu bar and choose the option 'Export table'  
- Choose the following settings:
  - Separator: Comma
  - Charset: UTF-8
  - Columns: Source, Target, Type, Id, Weight
- Click OK to start export; save the resulting csv file

Resulting files:
- [survey_presets_frequencies_edgematrix [Nodes].csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies_edgematrix%20%5BNodes%5D.csv)
- [survey_presets_frequencies_edgematrix [Edges].csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies_edgematrix%20%5BEdges%5D.csv)

## Step 4: Enhance node table with attributes 
Attributes to add:
- Label
- Weight
- Activity
- Phase
- TMIE
- GEO

- Take node table exported from Gephi ([survey_presets_frequencies_edgematrix [Nodes].csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/survey_presets_frequencies_edgematrix%20%5BNodes%5D.csv)) and modify/add columns as follows:
  - Replace names in column **Label** with unique tool names (e.g. Scopus (search); Scopus (impact); etc)
  - Add column **Weight** with frequency data for each tool (format: 18800.0)
  - Add column **Activity** with research activity as mentioned in the survey (Search, Access, Alerts, etc)
  - Add column **Phase** with research phase as mentioned in the survey (Discovery, Analysis, Writing, Publication, Outreach, Assessment)
  - Add column **TMIE** with TMIE-classification of each tool (Traditional, Modern, Innovative, Experimental)
  - Add column **GEO** with GEO-classification of each tool (Good, Efficient, Open, or a combination of two of these)

This step should optimally be coded in R as well, but for now, I did it in Calc with VLOOKUP and some manual adjustments for unique tool names. 

Attributes can be retrieved from:
- survey data (cleaned) (**Weight**) (from [Zenodo] (http://dx.doi.org/10.5281/zenodo.49583) or [Kaggle] (https://www.kaggle.com/bmkramer/101-innovations-research-tools-survey))
- survey variable list (**Label**, **Activity**) (from [Zenodo] (http://dx.doi.org/10.5281/zenodo.49583) or [Kaggle] (https://www.kaggle.com/bmkramer/101-innovations-research-tools-survey))
- [tools database] (http://bit.ly/innoscholcomm-list) (**Phase**, linked to Activity)
- [coupling_variables_toolID_TMIE_GEO.csv] (https://github.com/bmkramer/101innovations-survey-data/blob/network_viz/coupling_variables_toolID_TMIE_GEO.csv) (**Label**, **TMIE**, **GEO**)

Resulting file: 

## Step 5: Import enhanced node table into Gephi
- In Gephi, select the tab Data Laboratory and choose 'Import spreadsheet'
- In the first dialog screen (**General options**), select the csv to upload, choose the following settings and click Next:
  - Separator: Comma
  - As table: Nodes table
  - Charset: UTF-8
- In the next dialog screen (**Import settings**), choose the following settings and click Finish:
  - All columns should be checked to idicate they are to be imported (default setting)
  - All columns except **Weight** should be of the type **String** (default setting)
  - Set type of column **Weight** to **Double**
  - Option 'Force nodes to be created as new ones' should be unchecked (default setting)

The node table will now be imported and all existing nodes will be updated with the new information. 

## Step 6: Create network visualization in Gephi

