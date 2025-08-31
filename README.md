# GitHub Topic Scraper Project

A comprehensive web scraping tool that extracts popular topics and their top repositories from GitHub, providing insights into trending projects across various technology domains with interactive Power BI visualization.

## 📋 Project Overview

This project scrapes GitHub's topics page to collect information about popular programming topics and their associated repositories. It automatically generates detailed datasets for analysis and creates an interactive Power BI dashboard for comprehensive data visualization and business intelligence insights.

## 🚀 Features

- **Topic Discovery**: Scrapes all topics from GitHub's topics page
- **Repository Analysis**: Extracts top repositories for each topic including:
  - Repository name and owner
  - Star count
  - Repository URL
  - Topic classification
- **Data Export**: Generates individual CSV files for each topic
- **Data Consolidation**: Merges all topic data into a single comprehensive dataset
- **Visualization**: Creates charts and graphs to analyze repository popularity
- **📊 Power BI Dashboard**: Interactive business intelligence dashboard for advanced analytics

## 📊 Data Collected

For each topic, the scraper collects:
- **Topic Information**: Title, description, and URL
- **Repository Details**: 
  - Username/Organization
  - Repository name
  - Star count (with parsing for 'k' notation)
  - Repository URL
  - Topic classification

## 🛠️ Technologies Used

- **Python Libraries**:
  - `requests` - Web page downloading
  - `BeautifulSoup` - HTML parsing and data extraction
  - `pandas` - Data manipulation and CSV operations
  - `matplotlib.pyplot` - Data visualization
  - `glob` - File operations
  - `os` - Operating system interface

- **Business Intelligence**:
  - **Power BI Desktop** - Interactive dashboard creation
  - **Power BI Service** - Dashboard sharing and collaboration

## 📁 Project Structure

```
github-scrapping-project/
├── github topic scraper project.ipynb    # Main scraper implementation
├── final github scrapping project.ipynb  # Organized final version
├── getting one csv/
│   ├── main.ipynb                        # CSV consolidation script
│   └── README.md                         # Consolidation tool documentation
├── all csv files/                        # Individual topic CSV files
│   ├── 3D.csv
│   ├── Ajax.csv
│   ├── Algorithm.csv
│   ├── Amazon Web Services.csv
│   ├── Amp.csv
│   ├── Android.csv
│   ├── Angular.csv
│   ├── ansible.csv
│   ├── API.csv
│   ├── Arduino.csv
│   ├── ASP.NET.csv
│   ├── Awesome Lists.csv
│   ├── Azure.csv
│   ├── Babel.csv
│   ├── Bash.csv
│   ├── Bitcoin.csv
│   ├── Bootstrap.csv
│   ├── Bot.csv
│   ├── C.csv
│   ├── C++.csv
│   ├── Chrome extension.csv
│   ├── Chrome.csv
│   ├── Clojure.csv
│   ├── Code quality.csv
│   ├── Code review.csv
│   ├── Command-line interface.csv
│   ├── Compiler.csv
│   ├── Continuous integration.csv
│   ├── Cryptocurrency.csv
│   ├── Crystal.csv
│   └── topics.csv
├── merged_data.csv                       # Consolidated dataset
├── github_dashboard.pbix                 # Power BI Dashboard file
└── README.md                             # This documentation
```

## 🔧 Installation and Usage

### Prerequisites
```bash
pip install requests beautifulsoup4 pandas matplotlib
```

### Running the Main Scraper

Execute the main scraping notebook:

```python
# Import required libraries
import requests
from bs4 import BeautifulSoup
import pandas as pd
import os
import matplotlib.pyplot as plt

# Main scraping functions
def scrape_topics_repos():
    print("Scraping list of topics:")
    topics_df = scrape_topics()
    for index, row in topics_df.iterrows():
        print('Scraping top repositories for "{}"'.format(row['title']))
        scrape_topic(row['url'], row['title'])

# Run the complete scraper
scrape_topics_repos()
```

### Data Consolidation

Navigate to `getting one csv/` folder and run:

```python
import pandas as pd
import glob
import os

# Folder containing your CSV files
folder_path = r"D:\github scrapping project\all csv files"

# Get a list of all CSV files in the folder
csv_files = glob.glob(os.path.join(folder_path, '*.csv'))

# List to hold dataframes
dataframes = []

# Loop over each CSV file
for file in csv_files:
    # Read CSV file into a DataFrame
    df = pd.read_csv(file)
    
    # Add a new column for the filename (without the path and extension)
    df['filename'] = os.path.basename(file).replace('.csv', '')
    
    # Append the DataFrame to the list
    dataframes.append(df)

# Concatenate all dataframes into a single DataFrame
merged_df = pd.concat(dataframes, ignore_index=True)

# Save the merged DataFrame to a CSV file
merged_df.to_csv('merged_data.csv', index=False)

print("Merged CSV file created as 'merged_data.csv'")
```

### Power BI Dashboard

1. Open `github_dashboard.pbix` in Power BI Desktop
2. Refresh data connection to `merged_data.csv`
3. Explore interactive visualizations and insights
   <img width="1325" height="749" alt="image" src="https://github.com/user-attachments/assets/78d646d2-ccce-46df-ad0c-254598644233" />


## 📊 Key Functions

### Web Scraping Functions

```python
def get_topic_titles(soup):
    """Extract topic titles from GitHub topics page"""
    topic_title_tags = soup.find_all('p', {"class": "f3 lh-condensed mb-0 mt-1 Link--primary"})
    return [tag.text for tag in topic_title_tags]

def get_topic_descs(soup):
    """Extract topic descriptions"""
    description_class = {"class": "f5 color-fg-muted mb-0 mt-1"}
    topic_description_class = soup.find_all('p', description_class)
    return [tag.text.strip() for tag in topic_description_class]

def get_repo_info(h3_tag, star_tag):
    """Extract repository information"""
    a_tag = h3_tag.find_all('a')
    user_name = a_tag[0].text.strip()
    repo_name = a_tag[1].text.strip()
    repo_url = base_url + a_tag[1]['href']
    stars = parse_star_strip(star_tag.text.strip())
    return user_name, repo_name, repo_url, stars

def parse_star_strip(star_str):
    """Convert star count strings (e.g., '10.5k') to integers"""
    star_str = star_str.strip()
    if star_str[-1] == 'k':
        return int(float(star_str[:-1]) * 1000)
    return int(star_str)
```

## 📈 Data Visualization Features

### Python Visualizations
```python
# Create comprehensive charts
plt.figure(figsize=(16, 7))

# Bar Chart
plt.subplot(1, 2, 1)
bars = plt.bar(topic_repo_df['repo_name'], topic_repo_df['stars'], color='skyblue')
plt.xlabel('Repository Name')
plt.ylabel('Stars')
plt.title('Stars for Each Repository')

# Pie Chart
plt.subplot(1, 2, 2)
plt.pie(topic_repo_df['stars'], labels=None, startangle=140)
plt.title('Share of Stars by Repository')
```

### Power BI Dashboard Features

The interactive dashboard provides:

#### 📈 Key Visualizations
- **Repository Trends**: Time-series analysis of repository popularity
- **Topic Comparison**: Side-by-side comparison of different technology topics
- **Star Distribution**: Visualization of star counts across repositories
- **Top Contributors**: Leading users and organizations by repository count
- **Technology Landscape**: Market share and adoption patterns

#### 🔍 Interactive Features
- **Filter by Topic**: Drill down into specific technology areas
- **Search Functionality**: Find specific repositories or users
- **Dynamic Charts**: Real-time updates based on selected filters
- **Export Options**: Generate reports and export visualizations
- **Cross-filtering**: Interactive selection across multiple charts

## 📋 Sample Data Output

**Individual Topic CSV**:
```csv
user_name,repo_name,repo_url,stars
mrdoob,three.js,https://github.com/mrdoob/three.js,102000
pmndrs,react-three-fiber,https://github.com/pmndrs/react-three-fiber,27400
```

**Consolidated Dataset**:
```csv
user_name,repo_name,repo_url,stars,filename
mrdoob,three.js,https://github.com/mrdoob/three.js,102000,3D
microsoft,vscode,https://github.com/microsoft/vscode,164000,Code quality
flutter,flutter,https://github.com/flutter/flutter,166000,Android
```

## 🎯 Use Cases

- **Technology Trend Analysis**: Identify popular frameworks and tools
- **Business Intelligence**: Strategic technology adoption decisions
- **Open Source Discovery**: Find trending repositories by topic
- **Market Research**: Analyze technology adoption patterns  
- **Developer Insights**: Understand community preferences
- **Educational Resources**: Discover learning materials by topic
- **Investment Analysis**: Evaluate technology sector trends

## 📊 Dataset Statistics

- **30+ Technology Topics** covered including:
  - 3D, Ajax, Algorithm, Amazon Web Services, Angular, API, Arduino
  - ASP.NET, Awesome Lists, Azure, Babel, Bash, Bitcoin, Bootstrap
  - Bot, C, C++, Chrome, Chrome Extension, Clojure, Code Quality
  - Code Review, Command-line Interface, Compiler, Continuous Integration
  - Cryptocurrency, Crystal
- **600+ Repositories** analyzed
- **Complete Repository Metadata** including stars, URLs, and ownership
- **Historical Snapshot** of GitHub trends
- **Interactive Power BI Dashboard** with 15+ visualizations

## 🤝 Contributing

1. Fork the repository
2. Add new features or topics
3. Improve data visualization
4. Enhance Power BI dashboard
5. Submit pull requests

## 📝 Notes

- The scraper respects GitHub's structure and uses appropriate delays
- Data is collected from publicly available information
- Repository star counts are converted from GitHub's display format (e.g., "10.5k" → 10500)
- Individual CSV files allow for topic-specific analysis
- Power BI dashboard requires Power BI Desktop (free download from Microsoft)

## 🚀 Future Enhancements

- Add repository creation dates
- Include programming language detection
- Implement trending score calculation
- Add real-time data updates
- Expand Power BI dashboard with AI insights
- Create automated report generation
- Add predictive analytics for trending topics

## 🎨 Power BI Dashboard Preview

The dashboard includes:
- 📊 Executive summary with key metrics
- 🔍 Detailed repository analysis
- 📈 Trending topics visualization
- 👥 Top contributors and organizations
- 🌍 Technology landscape overview
- 📱 Mobile-responsive design

## 🔄 Complete Workflow

1. **Data Collection**: Run main scraper to extract topics and repositories
2. **Data Processing**: Execute consolidation script to merge CSV files
3. **Visualization**: Load data into Power BI for interactive analysis
4. **Insights**: Generate reports and discover technology trends

---

**Created for educational and research purposes to analyze GitHub's open source ecosystem with professional business intelligence
