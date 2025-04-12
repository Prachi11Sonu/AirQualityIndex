import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df = pd.read_csv("Quality_index.csv")
#Objective 1: Top 10 Most Polluted Cities (by Average AQI)seaborn graph

top_cities = df.groupby("city")["pollutant_avg"].mean().sort_values(ascending=False).head(10)

plt.figure(figsize=(10, 6))
plt.barh(top_cities.index[::-1], top_cities.values[::-1], color='darkred')
plt.title("Top 10 Most Polluted Cities (Avg AQI)")
plt.xlabel("Average AQI")
plt.tight_layout()
plt.show()

# Objective 2: Distribution of Pollutant Types pie chart

df.columns = df.columns.str.strip().str.lower().str.replace(" ", "_")
pollutant_counts = df['pollutant_id'].value_counts()
plt.figure(figsize=(8, 8))
plt.pie(
    pollutant_counts,
    labels=pollutant_counts.index,
    autopct='%1.1f%%',
    startangle=140,
    colors=plt.cm.Paired.colors
)
plt.title("Distribution of Pollutant Types (Pie Chart)")
plt.axis('equal')
plt.tight_layout()
plt.show()


#Objective 3: Pollution Trends by State (Average AQI)bar graph
state_avg = df.groupby("state")["pollutant_avg"].mean().sort_values(ascending=False).head(10)

plt.figure(figsize=(10, 6))
state_avg.plot(kind='bar', color='teal')
plt.title("Top 10 States by Average AQI")
plt.ylabel("Average AQI")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

#Objective 4: Heatmap Pollutant Averages by City and Type Heatmap
pivot = df.pivot_table(values="pollutant_avg", index="city", columns="pollutant_id", aggfunc="mean").fillna(0)
top_cities_heatmap = pivot.sort_values(by=pivot.columns.tolist(), ascending=False).head(10)

plt.figure(figsize=(12, 6))
sns.heatmap(top_cities_heatmap, cmap="YlOrRd", annot=True, fmt=".1f")
plt.title("Heatmap: Top 10 Cities by Pollutant Type (Average)")
plt.tight_layout()
plt.show()

# Objective 5: Correlation Between Pollutant Min, Max, and Avg
plt.figure(figsize=(8, 6))
sns.heatmap(df[["pollutant_min", "pollutant_max", "pollutant_avg"]].corr(), annot=True, cmap="coolwarm")
plt.title("Correlation Matrix Between Pollutant Levels")
plt.tight_layout()
plt.show()
