# Blackjack Analytics Pipeline (AWS + PySpark)

End-to-end **cloud-based big data analytics pipeline** using **AWS (S3, EC2, IAM, SageMaker)** and **PySpark** to ingest, process, analyze, and store blackjack game data for downstream analytics, machine learning, and dashboards.

## Project Overview
This project builds a production-style data pipeline:
- Ingest raw blackjack hands data from **Amazon S3**
- Process and feature-engineer using **PySpark** on **EC2**
- Write processed outputs back to **S3** in **CSV** format
- Run analytics using **Spark SQL** for game insights
- Train machine learning models using **SageMaker Canvas**
- Create interactive dashboards via **Power BI**

## Dataset
**900,000 Hands of Blackjack Results Dataset** from Kaggle.  
[Dataset Link](https://www.kaggle.com/datasets/mojocolors/900000-hands-of-blackjack-results/data)

Key fields: `PlayerNo, card1-5, sumofcards, dealcard1-5, sumofdeal, blkjck, winloss, plybustbeat, dlbustbeat, plwinamt, dlwinamt, ply2cardsum`.

> Note: This repo contains only a sample for demo. Full dataset should be stored in S3 bucket `s3a://blackjack-900-ujjwal/raw/`.

## Architecture
1. **S3 (raw + processed)** stores the dataset and outputs
2. **EC2 (Linux)** runs PySpark jobs for processing/analytics
3. **PySpark** performs cleaning, feature engineering, aggregation
4. **Spark SQL** runs analytical queries (win rates, bust rates, player performance, etc.)
5. **SageMaker Canvas** used for no-code model training & evaluation
6. **Power BI** dashboard created from processed dataset

![Architecture](Architecture/S3%20Bucket.png)

## Features Implemented
### Data Processing (PySpark)
- Drop unnecessary columns (`_c0`)
- Feature engineering:
  - `playerTotalCards` = count of cards drawn by player (card1-5 > 0)
  - `dealerTotalCards` = count of cards drawn by dealer (dealcard1-5 > 0)
- Data validation and schema inference

### Storage
- Output formats:
  - **CSV** (BI-friendly, stored in `s3a://blackjack-900-ujjwal/processed/`)
- Organized S3 structure:
  - `raw/` - original dataset
  - `processed/` - cleaned and feature-engineered data

### Analytics (Spark SQL & PySpark Aggregations)
- **Game Statistics**:
  - Average player vs dealer final card sums
  - Player win rate (42.88%)
  - Dealer win rate (47.76%)
  - Blackjack frequency (4.78% of hands)
  - Player bust rate (17.87%)
  - Dealer bust rate (28.11%)

- **Player Performance**:
  - Top 10 players by total winnings
  - Win rate by number of cards drawn
  - Average win amounts by win type

- **Game Patterns**:
  - Most common final totals (20 is most frequent)
  - Distribution of initial 2-card sums
  - Average cards drawn by dealer based on game outcome

### Machine Learning (SageMaker Canvas)
- Model training and evaluation on processed blackjack data
- Predictive analytics for game outcomes

## Repo Structure
```
AWS/
├── Architecture/          # Architecture diagrams
│   └── S3 Bucket.png
├── data/                  # Sample data files
│   ├── blkjckhands.csv
│   └── Proceed data/      # Processed output samples
├── Pyspark Code/          # Main PySpark notebook
│   └── Bigdata.ipynb
├── Outputs/               # Analytics outputs and screenshots
│   ├── AWS sagemaker/     # SageMaker model screenshots
│   ├── Metrics/           # Analytics metrics visualizations
│   └── SQL/               # SQL query result screenshots
├── PowerBI/               # Power BI dashboard files
└── QuickSuite/            # QuickSight related files
```

## Quickstart (Local)
```bash
# Install dependencies
python -m venv .venv
source .venv/bin/activate
pip install -U pip pyspark findspark

# Configure Spark (adjust path as needed)
# Ensure Spark is installed at /opt/spark or update path in notebook

# Run Jupyter notebook
jupyter notebook "Pyspark Code/Bigdata.ipynb"
```

### AWS Setup
1. **S3 Bucket**: Create bucket `blackjack-900-ujjwal` with folders:
   - `raw/` - upload `blkjckhands.csv`
   - `processed/` - for outputs

2. **EC2 Instance**: 
   - Launch EC2 instance with Spark installed
   - Configure IAM role with S3 read/write permissions
   - Update S3 bucket name in notebook if different

3. **Credentials**: Configure AWS credentials using:
   - IAM roles (recommended for EC2)
   - Or AWS credentials file

## Results & Insights
Analytics reveal:
- **Player Advantage**: Players win 42.88% of hands, dealers win 47.76%
- **Bust Analysis**: Dealers bust more frequently (28.11%) than players (17.87%)
- **Card Strategy**: Win rate decreases as players draw more cards:
  - 2 cards: 52.66% win rate
  - 3 cards: 36.92% win rate
  - 4 cards: 27.29% win rate
  - 5 cards: 25.21% win rate
- **Blackjack Frequency**: Natural blackjack (21 with 2 cards) occurs in 4.78% of hands
- **Most Common Total**: Final total of 20 is the most frequent outcome (146,800 occurrences)
- **Top Performers**: Player6 leads with total earnings of 1,462,085

## SageMaker Canvas Results

Machine learning models were trained using SageMaker Canvas for predictive analytics on game outcomes.

![SageMaker Results](Outputs/AWS%20sagemaker/1.png)

## License
MIT License

