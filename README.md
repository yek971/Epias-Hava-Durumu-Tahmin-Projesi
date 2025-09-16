# Epias-Hava-Durumu-Tahmin-Projesi

# EPİAŞ + ENTSO-E + Open-Meteo D+1 Energy Price Forecasting Project ## 📁 Project Structure
Project Main/
│
├── run_epias_features.py
├── requirements.txt
├── .env                # Holds EPIAS_EMAIL, EPIAS_PASSWORD, etc.
│
├── data/
│   ├── raw/
│   ├── interim/
│   ├── processed/
│   └── debug/          # Saves raw JSON API responses for debugging
│
├── src/
│   ├── __init__.py
│   ├── etl/
│   │   ├── __init__.py
│   │   ├── epias_client.py        # Legacy client (deprecated)
│   │   └── epias_tgt_client.py    # ✅ New TGT-authenticated client
│   │
│   └── features/
│       ├── __init__.py
│       └── build_features.py      # Builds feature tables from EPİAŞ data
│
└── notebooks/           # (optional) for exploration
## ⚙️ Setup 1. Create & activate virtual environment
bash
   python -m venv .venv
   .venv\Scripts\activate   # (Windows)
2. Install requirements
bash
   pip install -r requirements.txt
3. Create .env file in project root:
EPIAS_EMAIL=your_email
   EPIAS_PASSWORD=your_password
## ▶️ Running the pipeline
bash
cd "C:\Users\ozkan\OneDrive\Desktop\Project Main"
set EPIAS_START=2025-09-01
set EPIAS_END=2025-09-07
python run_epias_features.py
Output → data/processed/epias_features.csv ## ✅ Working Endpoints - day_ahead_mcp_es → Gün Öncesi Piyasa Takas Fiyatı (PTF) - system_marginal_price_es → Sistem Marjinal Fiyat (SMF) - realtime_generation_es → Gerçekleşen üretim (mix) - (Next: add consumption_actual, day_ahead_consumption, etc.) ## ⏭️ TODO (Next Steps) - [ ] Add **consumption_actual** & **day_ahead_consumption** with TGT client - [ ] Merge load & generation data with PTF/SMF - [ ] Add **feature engineering**: - Lag features (t-1, t-24, rolling mean) - Calendar vars (hour, dayofweek, holiday) - Weather features (from Open-Meteo) - [ ] Train baseline model (LightGBM) - [ ] Build **Streamlit dashboard** ## 🔑 Useful Commands Check environment vars:
bash
python check_env.py
Quick test PTF fetch:
bash
python -c "from src.etl.epias_tgt_client import day_ahead_mcp_es; import pandas as pd; df=day_ahead_mcp_es('2025-09-01','2025-09-07'); print(df.head()); print('rows:', len(df))"
