# Southern Oscillation Reproduction Project

## Overview

This project reproduces the landmark 1981 study by John D. Horel and John M. Wallace on "Planetary-Scale Atmospheric Phenomena Associated with the Southern Oscillation" using modern ERA-5 reanalysis data from Copernicus Climate Data Store.

## Original Study Context

The original paper (published in Monthly Weather Review, April 1981) examined:
- Interannual variability of seasonal means during Northern Hemisphere winter (1951-78)
- Sea surface temperature (SST) in equatorial Pacific
- Precipitation at equatorial Pacific stations
- Sea level pressure (Southern Oscillation Index)
- 200 mb and 700 mb geopotential height fields
- Tropospheric temperature at stations throughout the tropics
- Teleconnections to Northern Hemisphere extratropical latitudes

## Project Objectives

1. **Reproduce Key Visualizations**: Create modern equivalents of all major figures from the original paper
2. **Use Modern Data**: Leverage ERA-5 reanalysis data (1950-present) for consistency and completeness
3. **Demonstrate Climate Patterns**: Show the Southern Oscillation's global teleconnection patterns
4. **Educational Tool**: Provide an interactive Jupyter notebook for learning about climate variability

## Data Sources

### ERA-5 Reanalysis (Copernicus Climate Data Store)
- **Geopotential Height**: 200 hPa, 700 hPa, 1000 hPa levels
- **Sea Surface Temperature**: Monthly means
- **Sea Level Pressure**: Monthly means
- **Surface Air Temperature**: 2m temperature
- **Precipitation**: Total precipitation
- **Time Period**: 1950-1978 (to match original study), extensible to present

### Station Data
The study focuses on specific tropical Pacific stations:
- **Primary 200 mb stations**: Hilo, Tahiti, Nairobi, Darwin, San Juan, Lima, Guam, Nandi, Cocos Island, Abidjan
- **Rainfall stations**: Tarawa, Canton, Christmas, Fanning

## Key Analyses to Reproduce

### 1. Sea Surface Temperature Analysis (Figs. 1-2)
- Empirical Orthogonal Function (EOF) analysis of equatorial Pacific SST
- First eigenvector spatial pattern
- Time series of principal component
- Normalized seasonal anomaly time series

### 2. Precipitation Patterns (Fig. 3)
- Time series of December-February rainfall at equatorial island stations
- Correlation with SST patterns
- Identification of warm/cool episodes

### 3. Southern Oscillation Index (Fig. 4)
- Sea level pressure difference (Tahiti minus Darwin)
- Normalized time series
- Relationship to warm episodes

### 4. Upper Troposphere 200 mb Height (Figs. 5-8)
- Station locations map
- Time series at 10 primary tropical stations
- Composite Index from normalized anomalies
- Spatial composite maps for positive/negative episodes

### 5. Teleconnection Patterns (Figs. 9-11)
- Correlation maps: 700 mb height with SST, rainfall, SLP, 200 mb height
- Pacific/North American (PNA) pattern index
- West Pacific (WP) pattern index
- Schematic global teleconnection diagram

## Technical Components

### MCP Server for ERA-5 Data
Located in `mcp_server/`, this component:
- Provides programmatic access to Copernicus Climate Data Store
- Handles authentication with user-provided API key
- Downloads monthly mean data at specified pressure levels
- Caches data locally to avoid redundant downloads
- Supports spatial subsetting and temporal slicing

### Jupyter Notebook
Located in `notebooks/southern_oscillation_reproduction.ipynb`:
- **Setup cells**: Import libraries, configure data access
- **Data loading**: Download and prepare ERA-5 data
- **Analysis cells**: One cell per major analysis/figure
- **Visualization cells**: Reproduce each figure from the paper
- **Interactive elements**: Allow users to modify time periods, regions, thresholds

### Custom Skills
Located in `skills/`:
- **Climate Data Analysis**: EOF analysis, correlation maps, composite analysis
- **Teleconnection Visualization**: Correlation plots, spatial patterns, time series
- **Statistical Methods**: Normalization, seasonal averaging, significance testing

## Methodology

### Data Processing Pipeline
1. **Download**: Fetch ERA-5 monthly means via MCP server
2. **Quality Control**: Check for missing data, outliers
3. **Spatial Averaging**: Compute area-weighted means for regions
4. **Temporal Averaging**: Create seasonal (DJF) means
5. **Normalization**: Remove climatology, divide by standard deviation
6. **Analysis**: EOF, correlation, composite, statistical tests

### Warm Episode Definition
Following the original paper:
- Above-normal SST in equatorial Pacific
- Above-normal sea level pressure in Australia-Indonesia
- Weakening of subtropical high in Southeast Pacific
- Reversal of easterly winds in equatorial central Pacific
- Sharply enhanced precipitation at equatorial stations
- Enhancement of Hadley circulation in Pacific sector
- Teleconnections to extratropical latitudes

### Time Periods
- **Primary Analysis**: 1951-1978 (28 years, matching original study)
- **Seasonal Focus**: DJF (December-January-February) Northern Hemisphere winter
- **Warm Episodes Identified**: 1951-53, 1957-59, 1963-64, 1965-66, 1969-70, 1972-73, 1977-78

## Repository Structure

```
southern-oscillation-demo/
├── CLAUDE.MD                          # This file
├── README.md                          # User-facing documentation
├── environment.yml                    # Conda environment specification
├── requirements.txt                   # Python dependencies
├── mcp_server/                        # MCP server for ERA-5 data access
│   ├── __init__.py
│   ├── era5_server.py                # Main MCP server implementation
│   ├── config.json                   # MCP server configuration
│   └── README.md                     # Setup instructions
├── notebooks/                         # Jupyter notebooks
│   └── southern_oscillation_reproduction.ipynb
├── skills/                            # Custom analysis and visualization skills
│   ├── climate_analysis/
│   │   └── SKILL.md                  # Climate data analysis methods
│   └── teleconnection_viz/
│       └── SKILL.md                  # Teleconnection visualization methods
├── data/                              # Downloaded data (gitignored)
│   ├── era5/
│   ├── station/
│   └── processed/
├── figures/                           # Generated figures
│   └── reproductions/
└── tests/                             # Unit tests
    └── test_analysis.py
```

## Installation & Setup

### Prerequisites
- Python 3.9+
- Copernicus Climate Data Store (CDS) account and API key
- Conda or pip for package management
- Jupyter notebook environment

### Setup Steps
1. Clone repository
2. Create Python environment: `conda env create -f environment.yml`
3. Configure CDS API key: Add to `~/.cdsapirc` or set in notebook
4. Start MCP server: `python mcp_server/era5_server.py`
5. Launch Jupyter: `jupyter notebook notebooks/`

## Usage

### Quick Start
```python
# In Jupyter notebook
from mcp_server import ERA5Client

# Initialize with your API key
client = ERA5Client(api_key="YOUR_CDS_API_KEY")

# Download data for analysis period
data = client.download_monthly_means(
    variables=['geopotential', 'sea_surface_temperature'],
    pressure_levels=[200, 700],
    years=range(1951, 1979),
    months=range(1, 13)
)
```

### Reproducing Figures
Each major figure from the paper has a dedicated notebook cell:
- **Cell 1**: Load and preprocess data
- **Cell 2**: Fig. 1 - SST EOF analysis
- **Cell 3**: Fig. 2 - Normalized SST time series
- **Cell 4**: Fig. 3 - Rainfall time series
- ... (one cell per figure)

### Customization
Users can modify:
- **Time periods**: Extend analysis to 2024
- **Regions**: Focus on specific ocean basins
- **Thresholds**: Adjust warm/cold episode criteria
- **Visualizations**: Change color schemes, map projections
- **Statistics**: Add additional significance tests

## Key Findings to Replicate

From the original paper:
1. **SST Pattern**: Large amplitudes along equatorial belt, explaining 23% of variance
2. **Precipitation**: Strong correlation with SST; warm episodes = enhanced rainfall
3. **200 mb Height**: Above-normal heights in tropics during warm episodes (~1K temperature increase)
4. **Teleconnections**: 
   - Negative 700 mb height anomalies across North Pacific
   - Positive anomalies over western Canada
   - Negative anomalies over southeastern United States
   - Pacific/North American (PNA) pattern
   - West Pacific (WP) pattern

## Scientific Context

### Historical Importance
This paper was seminal in:
- Documenting global-scale impacts of tropical Pacific SST variability
- Establishing teleconnection patterns for seasonal forecasting
- Providing theoretical basis for El Niño prediction
- Demonstrating atmosphere-ocean coupling mechanisms

### Modern Relevance
The Southern Oscillation (now ENSO - El Niño Southern Oscillation):
- Remains primary source of seasonal predictability
- Influences weather worldwide through teleconnections
- Critical for agriculture, water resources, disaster preparedness
- Central to climate change impact assessments

## Extensions & Future Work

Potential enhancements to this reproduction:
1. **Extended Time Period**: Analyze 1950-2024 for climate change impacts
2. **Daily Data**: Examine subseasonal variability using ERA-5 daily data
3. **Additional Variables**: Include winds, vertical motion, humidity
4. **Model Comparison**: Compare ERA-5 with other reanalyses (MERRA-2, JRA-55)
5. **Mechanistic Analysis**: Add budget calculations, wave diagnostics
6. **Machine Learning**: Apply modern ML techniques to pattern detection
7. **Interactive Dashboards**: Create web-based exploration tools
8. **Real-time Monitoring**: Adapt for operational ENSO monitoring

## References

### Original Paper
Horel, J. D., and J. M. Wallace, 1981: Planetary-Scale Atmospheric Phenomena Associated with the Southern Oscillation. *Monthly Weather Review*, **109**, 813-829.

### Related Works
- Walker, G. T., and E. W. Bliss, 1932: World Weather V. *Mem. Roy. Meteor. Soc.*, **4**, 53-84.
- Bjerknes, J., 1969: Atmospheric teleconnections from the equatorial Pacific. *Mon. Wea. Rev.*, **97**, 163-172.
- Wallace, J. M., and D. S. Gutzler, 1981: Teleconnections in the geopotential height field during the Northern Hemisphere winter. *Mon. Wea. Rev.*, **109**, 784-812.
- Trenberth, K. E., 1976: Spatial and temporal variations of the Southern Oscillation. *Quart. J. Roy. Meteor. Soc.*, **102**, 639-653.

### Data Sources
- ERA-5: Hersbach et al., 2020: The ERA5 global reanalysis. *Quart. J. Roy. Meteor. Soc.*, **146**, 1999-2049.
- CDS Documentation: https://cds.climate.copernicus.eu/

## Contributing

Contributions welcome! Areas for improvement:
- Additional validation against original results
- Optimization of data download/processing
- Enhanced visualizations
- Documentation improvements
- Bug fixes and error handling

## License

MIT License - See LICENSE file for details

## Contact

For questions or issues, please open a GitHub issue or contact the repository maintainer.

---

*Created as a demonstration of reproducing classic climate science papers with modern data and tools.*
