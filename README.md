# FitSync-AI: AI Health & Fitness Plan Generator

**Capstone Project for Kaggle's 5-Day AI Agents Intensive**

## 🎯 Project Overview

FitSync-AI is a multi-agent AI system that generates personalized, data-driven health and fitness plans. Unlike static fitness apps, FitSync-AI uses machine learning models trained on Kaggle fitness datasets and intelligent agent orchestration to deliver dynamic, adaptive workout and diet recommendations tailored to each user's profile, goals, and progress.

This project demonstrates a production-ready implementation of the concepts from the **[5-Day AI Agents Intensive](https://www.kaggle.com/learn-guide/5-day-agents)** course by Google, including multi-agent systems, tool integration, memory management, and evaluation frameworks.

---

## 📋 Problem Statement

**Challenge:** Most users track fitness data (steps, calories, workout logs) but struggle to convert that data into actionable, personalized recommendations. Existing fitness apps provide generic plans that don't adapt to individual progress, preferences, or changing circumstances.

**Solution:** FitSync-AI automates the creation of personalized fitness and diet plans by:
1. **Analyzing** user profiles (age, gender, fitness level, activity history, dietary preferences)
2. **Modeling** fitness state using ML classifiers trained on real Kaggle datasets
3. **Orchestrating** multi-agent workflows to generate, validate, and adapt plans
4. **Delivering** actionable daily schedules with real-time feedback

---

## 🏗 Architecture

### System Design
```
User Input → Profile Agent → Plan Generator Agent → Scheduler Agent → Frontend Display
                    ↓              ↓                     ↓
            (Feature Extraction) (ML Model Inference)  (Execution Plan)
```

### Core Components

#### **1. Backend (Python + Gemini AI)**
- **Agent Development Kit (ADK)**: Multi-agent orchestration using Google's ADK
- **ML Models**: Classification and regression models for fitness prediction
- **Tool Integration**: Custom Python tools for data processing, API calls, and plan generation
- **API Layer**: RESTful endpoints exposing agent capabilities

#### **2. Frontend (HTML + JavaScript)**
- User onboarding form (profile data collection)
- Generated plan visualization (workouts, meal prep, progress tracking)
- Real-time feedback dashboard
- Integration with backend API for dynamic updates

#### **3. Data Pipeline (Kaggle-based)**
- Source: Public Kaggle fitness/health datasets (e.g., fitness tracking data, BMI correlations)
- Preprocessing: Feature normalization, encoding, handling missing values
- Training: Classification model (fitness level: "beginner/intermediate/advanced")
- Deployment: Model weights saved and loaded in backend agents

---

## 🤖 Multi-Agent System

FitSync-AI implements **three primary agents** communicating via ADK:

### **Agent 1: Profile & Assessment Agent**
- **Role**: Parse user inputs and assess current fitness level
- **Tools**: Data validation tool, feature extraction tool, fitness scoring tool
- **Output**: Standardized user profile and initial fitness classification
- **Integration**: Calls trained ML model to predict fitness category

### **Agent 2: Plan Generator Agent**
- **Role**: Create personalized workout and diet plans based on user profile and goals
- **Tools**: Plan template tool, calorie calculator tool, workout database tool
- **Output**: Multi-week fitness and nutrition plan with daily breakdown
- **Logic**: Adjusts intensity/duration based on user's fitness level and goals

### **Agent 3: Scheduler & Progress Agent**
- **Role**: Convert plans into executable daily schedules and track adherence
- **Tools**: Calendar tool, progress tracker tool, feedback generator tool
- **Output**: Daily checklist, achievement badges, adaptive recommendations
- **Memory**: Maintains session state for user history and plan adjustments

---

## 📊 ML Model Integration

### **Dataset Source**
- **Kaggle Dataset**: Fitness tracking data (age, BMI, heart rate, steps, calories burned, workout frequency)
- **Features**: Age, Gender, BMI, Activity Level, Weekly Steps, Calories Burned, Sleep Hours, Workout Frequency
- **Target Variable**: Fitness Level (Classification: 0=Needs Improvement, 1=Moderate, 2=Fit)

### **Model Pipeline**
```python
1. Data Preprocessing → 2. Feature Engineering → 3. Train/Validation Split
4. Model Training (Random Forest / Gradient Boosting) → 5. Hyperparameter Tuning
6. Model Evaluation (Accuracy, F1-Score) → 7. Deployment as Agent Tool
```

### **Integration in Agents**
The fitness classification model is wrapped as a **tool** callable by the Profile Agent:
```python
def predict_fitness_level(age, bmi, activity_level, weekly_steps):
    # Load pre-trained model
    return model.predict([[age, bmi, activity_level, weekly_steps]])
```

---

## 🛠 Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend Language** | Python 3.x |
| **AI Agent Framework** | Google Agent Development Kit (ADK) / Gemini |
| **ML Framework** | Scikit-learn, Pandas, NumPy |
| **Frontend** | HTML5, Vanilla JavaScript |
| **API** | Flask / FastAPI |
| **Database** | SQLite / Firebase (optional) |
| **Deployment** | Google Cloud (optional) |
| **Data Source** | Kaggle Datasets |

---

## 📁 Project Structure

```
FitSync-AI/
├── backend/
│   ├── agents/
│   │   ├── profile_agent.py          # Profile parsing & assessment
│   │   ├── plan_generator_agent.py   # Workout & diet plan creation
│   │   └── scheduler_agent.py        # Execution scheduling & progress
│   ├── tools/
│   │   ├── data_tools.py             # Feature extraction, validation
│   │   ├── ml_tools.py               # Model inference, predictions
│   │   ├── planning_tools.py         # Plan generation logic
│   │   └── schedule_tools.py         # Calendar & reminder tools
│   ├── models/
│   │   ├── fitness_classifier.pkl    # Trained ML model
│   │   └── model_training.py         # Training pipeline
│   ├── api/
│   │   └── main.py                   # Flask API endpoints
│   └── config.py                     # Configuration & API keys
├── frontend/
│   ├── index.html                    # Main UI
│   ├── css/
│   │   └── style.css                 # Styling
│   └── js/
│       └── app.js                    # Frontend logic & API integration
├── data/
│   ├── fitness_data.csv              # Kaggle dataset
│   └── processed_data.csv            # Preprocessed dataset
├── README.md
├── requirements.txt
└── LICENSE
```

---

## 🚀 Quick Start

### **Prerequisites**
- Python 3.8+
- Kaggle API credentials (for dataset access)
- Google Cloud Project with Gemini API enabled

### **Installation**

```bash
# 1. Clone repository
git clone https://github.com/SujitYalmar/FitSync-AI.git
cd FitSync-AI

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up environment variables
echo "GOOGLE_API_KEY=your_key_here" > .env
echo "KAGGLE_USERNAME=your_username" >> .env
echo "KAGGLE_KEY=your_key" >> .env
```

### **Training the Model (Optional)**

```bash
python backend/models/model_training.py
```

This downloads the Kaggle fitness dataset, preprocesses it, trains the fitness classifier, and saves it as `fitness_classifier.pkl`.

### **Running the System**

```bash
# Start backend API
python backend/api/main.py

# In browser, navigate to
http://localhost:5000
```

---

## 📚 How It Works (End-to-End Flow)

1. **User Onboarding**: User enters profile (age, gender, fitness level, goals, dietary preferences)
2. **Profile Agent**: Validates input, extracts features, runs ML model to classify fitness level
3. **Plan Generator**: Creates 4-week personalized plan with daily workouts and meal suggestions
4. **Scheduler**: Converts plan into executable daily checklist with reminders
5. **Frontend Display**: Shows user the generated plan, tracks daily progress
6. **Progress Feedback**: Scheduler agent monitors adherence and adapts recommendations

---

## 🎓 Kaggle 5-Day Agents Intensive Alignment

This capstone project implements concepts from all 5 days of the course:

| Day | Topic | Implementation in FitSync-AI |
|-----|-------|-----------------------------|
| **Day 1** | Agent Basics & Fundamentals | Multi-agent orchestration framework |
| **Day 2** | Tools & Model Context Protocol | Custom Python tools for data processing, ML inference, planning |
| **Day 3** | Context & Memory Management | Session state management across agents, user history tracking |
| **Day 4** | Evaluation & Observability | Plan quality scoring, adherence metrics, agent decision logging |
| **Day 5** | Production & Deployment | API design, error handling, scalability considerations |

**Track**: Concierge Agent (Personal AI Fitness Coach)

---

## 📊 Key Features

✅ **Personalized Plans**: Data-driven recommendations based on individual fitness level
✅ **Multi-Agent Orchestration**: Specialized agents for profiling, planning, and scheduling
✅ **ML Model Integration**: Kaggle dataset-trained fitness classifier
✅ **Adaptive Feedback**: Progress tracking with dynamic plan adjustments
✅ **Tool Ecosystem**: Reusable tools for data processing, planning, and execution
✅ **API-First Design**: RESTful backend for easy frontend integration
✅ **Production Ready**: Error handling, logging, and configuration management

---

## 🔄 Future Enhancements

- [ ] Advanced ML models (Neural Networks for progression prediction)
- [ ] Integration with fitness wearable APIs (Fitbit, Apple Watch, Garmin)
- [ ] Real-time coach feedback via voice/chat agents
- [ ] A/B testing framework for plan effectiveness
- [ ] Multi-language support
- [ ] Mobile app (Flutter) for better UX
- [ ] Cloud deployment on Google Cloud Run

---

## 📖 References

- [Kaggle 5-Day AI Agents Intensive](https://www.kaggle.com/learn-guide/5-day-agents)
- [Google Agent Development Kit Docs](https://ai.google.dev/)
- [Gemini API Documentation](https://ai.google.dev/docs)
- [Scikit-learn ML Documentation](https://scikit-learn.org/)

---

## 📝 License

This project is licensed under the **MIT License** – see [LICENSE](./LICENSE) file for details.

---

## 👤 Author

**Sujit Yalmar**
- GitHub: [@SujitYalmar](https://github.com/SujitYalmar)
- Focus: Full-stack AI integration, Multi-agent systems, Cloud deployment
- Capstone Project: Kaggle 5-Day AI Agents Intensive

---

## 💡 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

---

**Built with ❤️ for the Kaggle 5-Day AI Agents Intensive Capstone**
