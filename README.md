# AI Feast on the Table for AU

An AI-powered personalized dining recommendation system built for the Asia University Generative AI Competition, combining real campus restaurant menus with OpenAI GPT.

After the user enters their gender, age, height, weight, and weekly exercise days, the system:

1. Calculates BMI, Basal Metabolic Rate (BMR), and Total Daily Energy Expenditure (TDEE)
2. Filters the campus restaurant menu database by BMI range and randomly recommends 3 dishes
3. Calls GPT-3.5-turbo to generate a personalized nutrition analysis and dietary advice in the voice of a nutritionist

## Project Structure

```
ai-feast-on-the-table/
├── api/
│   └── index.py            # Flask backend (Vercel Serverless Function)
├── public/                 # Static frontend
│   ├── index.html          # Welcome page
│   ├── homepage.html       # Main app page
│   └── images/             # Background images
├── data/
│   └── restaurant_information_v3.csv   # Restaurant menus with nutrition facts
├── scripts/
│   └── get_restaurant_information.py   # Menu scraper (QuickClick API)
├── docs/                   # Project proposal, architecture & flow diagrams
├── showcase/               # Demo results (kept local only, not in version control)
├── vercel.json             # Vercel routing config
├── requirements.txt
├── .env.example
└── LICENSE
```

## Local Development

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Set up environment variables
copy .env.example .env
# Edit .env and fill in your OPENAI_API_KEY

# 3. Start the app (Flask serves both the public/ static pages and the API)
python api/index.py
```

Then open <http://localhost:5000> in your browser.

## Deploying to Vercel

1. Sign in to [vercel.com](https://vercel.com) with your GitHub account and import this repository
2. Choose **Other** as the Framework Preset; no build command is needed
3. Add `OPENAI_API_KEY` under **Settings → Environment Variables**
4. Deploy — `public/` is served as static files by the CDN, and `/api/*` is automatically routed to `api/index.py`

## API

### `POST /api/recommend_food`

Example request:

```json
{
  "gender": "男性",
  "height": 175,
  "weight": 70,
  "age": 22,
  "exercise": 3
}
```

`gender` accepts `男性` (male) or `女性` (female); `exercise` is the number of exercise days per week (0–7).

The response includes the BMI, daily caloric need, recommended macronutrient intake, 3 recommended dishes, and a GPT-generated nutrition analysis.

## Data Source

`scripts/get_restaurant_information.py` scrapes menus and prices of 12 restaurants around Asia University via the QuickClick LINE store API.
Nutrition facts (calories, protein, fat, carbohydrates) were compiled manually and merged into `data/restaurant_information_v3.csv`.

## History

The original competition-era codebase had two instances (`localtest_instance` for local testing and `web_instance` for the campus server deployment) plus PHP/MySQL registration/login pages. These were removed during the restructuring in favor of a single Vercel-deployable version; the old code is still available in the git history (initial commit).

## License

This project is licensed under the [Apache License 2.0](LICENSE).
