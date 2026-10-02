# Reflection

## Copilot-Assisted and Version-Controlled BI Development

GitHub Copilot was useful during the DAX development stage because it provided a starting point for several measures. The suggestions for Total Sales, Month-over-Month Growth, Running Total Sales, Item Rank, and Average Transaction Value were directly usable after I checked them against the Power BI model and tested their results in visuals.

The Cold Brew Sales % measure required more careful review. Copilot initially treated Cold Brew as a value in the Category field, but Cold Brew is a specific item in this dataset. I corrected the filter to use the Item field and then tested the revised measure with report filters. This showed me that Copilot can speed up development, but its output still needs to be checked against the actual data model and business meaning.

Working with a full Git commit history also changed how I approached the project. Instead of building everything first and saving one final version, I developed the solution in smaller stages: repository setup, star schema, individual measures, documentation, and dashboard. Each commit represented a specific change, making the development process easier to review and helping me identify problems before moving to the next stage. Overall, the combination of Power BI, GitHub, and Copilot made the BI development process more structured and traceable.