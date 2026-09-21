# Air Aware AI

Build a modern, responsive web application called “AirAware AI – Air Quality Monitoring Assistant” for an AI for Sustainability Virtual Internship project.

Project Goal

Create an easy-to-use platform that helps users understand air quality in their location. The application should display AQI and major pollutant information, provide simple explanations, show historical trends, and give practical recommendations based on air-quality conditions.

The project should align with:

SDG 11 – Sustainable Cities and Communities

SDG 13 – Climate Action

Target Users

Students

General public

City residents

People who want to monitor local air pollution

Design

Create a clean, modern environmental dashboard.

Use:

White/light background

Green and blue as the primary visual theme

Soft cards with rounded corners

Simple icons

Clear typography

Responsive design for desktop, tablet, and mobile

Professional but student-friendly appearance

Subtle animations and hover effects

Avoid excessive gradients or complicated visual effects

Navigation

Create a top navigation bar with:

AirAware AI | Home | Air Quality | Trends | AI Assistant | About

Include a prominent “Check Air Quality” button.

1. Home Page

Create a visually attractive hero section.

Heading:

“Understand the Air You Breathe”

Subtitle:

“AirAware AI helps you monitor air quality, understand pollution levels, and make informed decisions for a healthier and more sustainable environment.”

Add buttons:

Check Air Quality

Ask AI Assistant

Include a clean environmental illustration or air-quality themed visual.

Below the hero section, display four feature cards:

Real-Time Air Quality
Monitor current AQI and pollutant levels.

Pollution Trends
Understand how air quality changes over time.

AI Insights
Get easy-to-understand explanations.

Health & Environment Tips
Receive recommendations based on air-quality conditions.

2. Air Quality Dashboard

Create a dedicated dashboard.

At the top, provide a location search field:

“Enter city or location”

Add a Search button.

Display a large AQI card containing:

AQI value

Air quality category

Location

Last updated time

Use different visual indicators for AQI categories:

Good

Moderate

Unhealthy for Sensitive Groups

Unhealthy

Very Unhealthy

Hazardous

Do not rely only on colors. Always display the category text clearly.

Below the AQI card, create pollutant cards for:

PM2.5

PM10

CO

NO₂

SO₂

O₃

Each card should show:

Current value

Unit

Simple status indicator

Add a section:

“What does this mean?”

Provide a simple explanation of the current AQI level.

3. Air Quality Trends

Create a Trends page.

Display interactive charts showing historical air-quality data.

Include:

AQI over time

PM2.5 over time

PM10 over time

Allow users to select:

Last 24 hours

Last 7 days

Last 30 days

Use clean line charts with tooltips.

Also display summary statistics:

Average AQI

Highest AQI

Lowest AQI

Average PM2.5

Add a section:

“Air Quality Insights”

Example:
“Air quality has improved compared with the previous period.”

Make this insight dynamic based on the available data.

4. AI Assistant

Create an AI chat interface called:

“AirAware AI Assistant”

Description:

“Ask questions about air quality, pollution, environmental conditions, and sustainable actions.”

Example questions:

“Is the current air quality good?”

“What does PM2.5 mean?”

“Why is air pollution dangerous?”

“What precautions should I take when AQI is high?”

“How can I reduce my contribution to air pollution?”

“Explain today's AQI in simple words.”

Create a chat interface with:

User messages

AI responses

Loading indicator

Clear chat button

Suggested questions

For the initial prototype, implement a safe rule-based/mock AI response system if an external AI API is not configured.

Structure the application so that an AI API such as IBM Granite through IBM watsonx.ai can be connected later.

The AI should use the current AQI and pollutant information when generating explanations.

Important:

Do not make medical diagnoses.

Do not claim that air quality data is a medical assessment.

Encourage users to follow local public-health guidance when appropriate.

Clearly communicate uncertainty when data is unavailable.

5. Recommendations

Create a section called:

“Smart Recommendations”

Recommendations should change according to AQI category.

Examples:

For good air quality:

Outdoor activities are generally suitable.

Continue sustainable transportation habits.

For moderate air quality:

Sensitive individuals should monitor conditions.

Consider reducing prolonged outdoor activity if pollution increases.

For poor air quality:

Consider limiting prolonged outdoor activity.

Monitor local air-quality updates.

Follow relevant public-health guidance.

Also include sustainability suggestions such as:

Use public transportation

Walk or cycle when conditions are suitable

Reduce unnecessary vehicle use

Avoid burning waste

Save energy

Support greener transportation

6. AQI Prediction – Optional Feature

Add a section called:

“AQI Forecast”

If historical data is available, use a simple machine-learning model to estimate future AQI.

Possible models:

Linear Regression

Random Forest

Decision Tree

Display:

Predicted AQI

Predicted category

Simple trend chart

Clearly label predictions as estimates, not guaranteed future values.

If no trained ML model or dataset is available, display sample/demo data and clearly label it as demonstration data.

7. About Page

Create an About page explaining the project.

Title:

“About AirAware AI”

Content:

“AirAware AI is an AI-assisted sustainability project designed to make air-quality information easier to understand and access. The platform combines environmental data, visualization, and AI-assisted explanations to help users understand pollution levels and make more informed decisions.”

Include:

Sustainability Goal

SDG 11 – Sustainable Cities and Communities

Climate Action

SDG 13 – Climate Action

Key Technologies

Python

Streamlit or modern web frontend

Machine Learning

Data Visualization

AI / IBM Granite

Environmental datasets or air-quality APIs

Add a section:

“AI for Sustainability”

Explain how AI can help analyze environmental data, identify patterns, communicate information clearly, and support sustainability awareness.

8. Data Handling

Design the application so that air-quality data can come from an API or uploaded dataset.

Create a clean data service layer.

The application should support fields such as:

timestamp

location

AQI

PM2.5

PM10

CO

NO₂

SO₂

O₃

If no API key is configured, provide realistic sample data so the complete website remains functional for demonstration.

Clearly label sample/demo data as Demo Data.

Do not present fabricated real-time data as actual live measurements.

9. Dashboard Features

Add:

Responsive layout

Search functionality

AQI status indicator

Pollutant cards

Historical charts

AI assistant

Recommendations

AQI prediction section

Data source/status indicator

Last updated timestamp

Loading states

Error states

Empty states

10. Footer

Create a professional footer containing:

AirAware AI

“Making environmental information easier to understand through AI and technology.”

Links:

Home

Air Quality

Trends

AI Assistant

About

Add:

SDG 11 | SDG 13

And:

“Built as an AI for Sustainability Virtual Internship Project.”

Technical Requirements

Use a clean component-based architecture.

Prefer:

React

TypeScript

Tailwind CSS

Reusable components

Responsive design

Accessible UI

Use a charting library for the graphs.

Keep the code organized into components such as:

Navbar

Hero

AQICard

PollutantCard

TrendChart

RecommendationCard

AIAssistant

Footer

Create mock data/services separately so they can easily be replaced with a real API later.

The application should run without requiring external API keys initially.

Make the final result look like a polished AI + Sustainability project suitable for an academic internship demonstration, rather than a generic template.

Important final requirement

After building the website, ensure that:

All navigation links work.

The AQI dashboard displays demo data.

Charts display correctly.

The AI assistant works with predefined/demo responses.

The website is responsive.

The application clearly distinguishes real data from demo data.

There are no broken buttons or empty sections.

The project can later be connected to an air-quality API and IBM Granite/watsonx.ai.

This project was built with [Lovable](https://lovable.dev).

**Live app**: https://enviro-ai-sense.lovable.app

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/55dc37de-8d6c-4ca8-b787-5ae33fdbe455).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```
