# Dashboard_with_GPT

# **Now we create a Dashboard within  10 minute using Chat GPT**

Requirements -  
            1 - Knowledge of Prompt
            
            2 - Account in chat_gpt
            
            3 - Dataset 
            
            4 - Domain Knowledge 


Phases of Project Execution -

            1- Open Chat gpt and login it 
            2 - Preparation of  Dataset 
            analysis 
            3 - Enter Prompt based on Dataset for Analysis 
            4 - Create a Dashboard .
                      4.a - generate a HTML code for Dashboard.
                      4.b - Downloadable html File 
                      4.c - Insert Dataset into Html File . 
                      
                      
            
            

 # 1- Open Chat gpt and login it  
   # **Yes**    ✔ 

 # **2 - Preparation of  Dataset**
 If dataset not in JSON  format . Then convert it into JSON format.
    Converter Tool Link  - https://csvjson.com/ 
   
 # 3 - Dataset analysis
    Now we Analyse Datsaset which is provided .
    Analyse Data type and field for better writing prompt.

    
    
    
# **4 - Enter Prompt based on Dataset for Analysis  for  recommand a chart **

    Also we import Sammple Dataset in json format for Better Result .\
    For Eg -  We use HR Dataset.  
 
**Prompt1** -  **This is the structure of my  dataset  in JSON format. 
            Please analyze it and recommend  six key data visualization metrics
           that would be ideal for building an interactive sales dashboard also provide legend of each . 
            Additionally, provide a brief explanation for the relevance of each metric to the  dashboard.**
            

When we Insert Sample dataset into Chat_gpt along with Above prompt 
  Result _    Six Key Data Visualization Metrics for an Interactive Sales Dashboard
Here are six metrics to visualize in your dashboard, including legends and their relevance:

**1. Attrition Rate by Department and Job Role
Visualization Type: Stacked Bar Chart**
Legend:
**Green: Active Employees
Red: Attrited Employees
Relevance: Understanding which departments and job roles experience the highest attrition helps identify areas that may require retention strategies.


**2. Employee Distribution by Distance from Home
Visualization Type: Histogram
Legend:
X-axis: Distance from Home (miles)
Y-axis: Number of Employees
Relevance: Analyzing commute distance can help address employee satisfaction and explore solutions like flexible working or office relocation.**


**3. Average Monthly Income by Job Role and Job Level
Visualization Type: Grouped Bar Chart
Legend:
Different colors represent distinct Job Roles
Relevance: Helps in benchmarking salaries and identifying inconsistencies that may affect morale or competitiveness.**


**4. Overtime Analysis vs Job Satisfaction
Visualization Type: Scatter Plot
Legend:
Blue: High Job Satisfaction
Yellow: Medium Job Satisfaction
Red: Low Job Satisfaction
Relevance: Evaluating the impact of overtime on job satisfaction reveals potential burnout issues and guides workload optimization.**


**5. Performance Rating vs Total Working Years
Visualization Type: Line Chart
Legend:
Solid Line: Average Performance Rating per Year
Relevance: Identifies trends in performance improvement or decline over employees' tenure to enhance career development initiatives.**

**6. Work-Life Balance and Training Times Last Year
Visualization Type: Heatmap
Legend:
Darker Shades: Higher Training Times
Lighter Shades: Lower Training Times**


#**Relevance:** Correlates work-life balance with training opportunities to identify areas where additional resources might enhance satisfaction.
Dashboard Explanation and Use - 
These metrics cover critical aspects such as attrition, compensation, workload, satisfaction, and professional growth. They are interconnected factors that influence employee engagement and sales productivity, making them ideal for actionable insights in a sales dashboard.**



# **  4 - Create a Dashboard ** - 
                    **   Let's Create Dashboard using Chat_gpt .
                       MAny step envolve to generate Dashboard.
              
  **4.a - generate a HTML code for Dashboard**
                        Now we Enter prompt to generate HTML code .
                        Prompt - Now, I want you to create an interactive sales performance dashboard with the five key visualizations using HTML and JavaScript. Use the latest version 
                       of Plotly.js CDN and include an upload feature so that we can upload the JSON file. It should generate the dashboard based on the uploaded data.
                       Note: I need to upload the sales data from my local computer, and it should create the dashboard based on that data.

**
**Result ** -  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Sales Performance Dashboard</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <h1>Sales Performance Dashboard</h1>
    <input type="file" id="upload" accept=".json" />
    <div id="visualizations">
        <div id="attrition-rate" style="width: 100%; height: 400px;"></div>
        <div id="distance-distribution" style="width: 100%; height: 400px;"></div>
        <div id="income-analysis" style="width: 100%; height: 400px;"></div>
        <div id="overtime-satisfaction" style="width: 100%; height: 400px;"></div>
        <div id="performance-tenure" style="width: 100%; height: 400px;"></div>
    </div>

    <script>
        document.getElementById('upload').addEventListener('change', function(event) {
            const file = event.target.files[0];
            const reader = new FileReader();

            reader.onload = function(e) {
                const data = JSON.parse(e.target.result);
                generateDashboard(data);
            };

            reader.readAsText(file);
        });

        function generateDashboard(data) {
            const departments = [...new Set(data.map(item => item.Department))];
            const jobRoles = [...new Set(data.map(item => item.JobRole))];

            // Attrition Rate by Department and Job Role
            const attritionData = departments.map(dept => {
                const filtered = data.filter(item => item.Department === dept);
                return {
                    x: jobRoles,
                    y: jobRoles.map(role => filtered.filter(item => item.JobRole === role && item.Attrition === 'Yes').length),
                    name: dept,
                    type: 'bar'
                };
            });
            
            Plotly.newPlot('attrition-rate', attritionData, {
                title: 'Attrition Rate by Department and Job Role',
                barmode: 'stack',
                xaxis: { title: 'Job Roles' },
                yaxis: { title: 'Number of Attritions' }
            });

            // Employee Distribution by Distance from Home
            const distances = data.map(item => item.DistanceFromHome);
            Plotly.newPlot('distance-distribution', [{
                x: distances,
                type: 'histogram',
            }], {
                title: 'Employee Distribution by Distance from Home',
                xaxis: { title: 'Distance from Home (miles)' },
                yaxis: { title: 'Number of Employees' }
            });

            // Average Monthly Income by Job Role
            const incomeData = jobRoles.map(role => {
                const roleData = data.filter(item => item.JobRole === role);
                return {
                    x: [...new Set(roleData.map(item => item.JobLevel))],
                    y: [...new Set(roleData.map(item => item.JobLevel))].map(level => {
                        const levelData = roleData.filter(item => item.JobLevel === level);
                        return levelData.reduce((sum, val) => sum + val.MonthlyIncome, 0) / levelData.length;
                    }),
                    type: 'bar',
                    name: role
                };
            });

            Plotly.newPlot('income-analysis', incomeData, {
                title: 'Average Monthly Income by Job Role and Job Level',
                barmode: 'group',
                xaxis: { title: 'Job Levels' },
                yaxis: { title: 'Average Monthly Income' }
            });

            // Overtime Analysis vs Job Satisfaction
            const satisfactionData = data.map(item => ({
                x: item.OverTime === 'Yes' ? 1 : 0,
                y: item.JobSatisfaction
            }));
            Plotly.newPlot('overtime-satisfaction', [{
                x: satisfactionData.map(d => d.x),
                y: satisfactionData.map(d => d.y),
                mode: 'markers',
                type: 'scatter'
            }], {
                title: 'Overtime vs Job Satisfaction',
                xaxis: { title: 'Overtime (1: Yes, 0: No)' },
                yaxis: { title: 'Job Satisfaction' }
            });

            // Performance Rating vs Total Working Years
            const performanceData = data.map(item => ({
                x: item.TotalWorkingYears,
                y: item.PerformanceRating
            }));

            Plotly.newPlot('performance-tenure', [{
                x: performanceData.map(d => d.x),
                y: performanceData.map(d => d.y),
                mode: 'lines+markers'
            }], {
                title: 'Performance Rating vs Total Working Years',
                xaxis: { title: 'Total Working Years' },
                yaxis: { title: 'Performance Rating' }
            });
        }
    </script>
</body>
</html>

#** I have created an HTML file for your interactive sales performance dashboard. 
   It uses the latest Plotly.js library and includes an upload feature that reads a JSON file and generates 
   the visualizations dynamically. You can save this file and open it in a browser to upload your data and view the dashboard. 
   Let me know if you need further modifications! 
**

# 4.b - Downloadable html File -
  ** Enter Prompt - Provide this as an HTML downloadable file? **
  Answer - You can download the interactive sales performance dashboard as an HTML file using the link below:

Download Interactive Sales Dashboard ​

            [Uploading interactive_sales_dashboard.html…]()

[Uploading interactive_sales_dashboard.html…]()



 # **4.c - Insert Dataset into Html File . -**
   Insert Dataset for showing Interactive Dashboard . 




            

