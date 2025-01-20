# Calorie Counter Project



## Overview
The Calorie Counter project is a simple web-based tool designed to help users track their daily calorie intake and expenditure. It allows users to input their calorie budget, log calories consumed during different meals, and account for calories burned through exercise. The app calculates the remaining calories based on the input and provides feedback on whether the user is within their budget, in a calorie deficit, or surplus.

## Features
Set a daily calorie budget.
Log calories for breakfast, lunch, dinner, snacks, and exercise.
Add multiple food or exercise entries for each category.
Calculate the remaining calories based on the budget and the logged data.
Clear all entries and reset the form.

## Technologies
HTML: Structure of the application, including forms, input fields, and buttons.
CSS: Styling to create a visually appealing user interface.
JavaScript: Handles the logic for adding entries, calculating calories, and displaying results.

## Installation
Download or clone the repository.
Open the index.html file in a web browser to start using the application.

## File Structure

    /CalorieCounterProject
        ├── index.html      # Main HTML file
        ├── styles.css      # CSS styles for the UI
        └── script.js       # JavaScript file handling app functionality


## How to Use
 - Set your Budget: Enter your daily calorie budget in the "Budget" input field.
 - Add Entries: Use the dropdown menu to select a category (Breakfast, Lunch, Dinner, Snacks, or Exercise). For each category, you can add food or exercise entries with their respective calorie values.
 - Calculate Remaining Calories: Once you've added your entries, click on "Calculate Remaining Calories" to see if you are in a calorie deficit or surplus.
 - Clear: Click on "Clear" to reset all fields and remove all entered data.

## Explanation of Key Functions

### addEntry()
This function adds a new entry for either food or exercise under the selected category (Breakfast, Lunch, Dinner, Snacks, or Exercise). It dynamically generates input fields for the name and calorie count of the item and appends them to the relevant category.
    
Example:
Adds a new "Entry" with two fields: one for the name of the food/exercise and another for its calorie value.

    function addEntry() {
        const targetInputContainer = document.querySelector(`#${entryDropdown.value} .input-container`);
        const entryNumber = targetInputContainer.querySelectorAll('input[type="text"]').length + 1;
        const HTMLString = `
        <label for="${entryDropdown.value}-${entryNumber}-name">Entry ${entryNumber} Name</label>
        <input type="text" id="${entryDropdown.value}-${entryNumber}-name" placeholder="Name" />
        <label for="${entryDropdown.value}-${entryNumber}-calories">Entry ${entryNumber} Calories</label>
        <input
          type="number"
          min="0"
          id="${entryDropdown.value}-${entryNumber}-calories"
          placeholder="Calories"
        />`;
        targetInputContainer.insertAdjacentHTML('beforeend', HTMLString);
    };`

### calculateCalories()
This function calculates the total calories consumed across all categories (breakfast, lunch, dinner, snacks) and compares it to the user's calorie budget. It also adds any calories burned through exercise to determine the remaining calories. The result is then displayed to the user, showing whether they are in a surplus or deficit.

Example:
- Collects all the calorie values from user inputs.
- Computes total calories consumed and burned.
- Displays whether the user is in a calorie surplus or deficit.

        function calculateCalories(e) {
          e.preventDefault();
          isError = false;

          // Get all calorie values from input fields
          const breakfastCalories = getCaloriesFromInputs(breakfastCalories);
          const lunchCalories = getCaloriesFromInputs(lunchNumberInputs);
          const dinnerCalories = getCaloriesFromInputs(dinnerNumberInputs);
          const snacksCalories = getCaloriesFromInputs(snacksNumberInputs);
          const exerciseCalories = getCaloriesFromInputs(exerciseNumberInputs);
          const budgetCalories = getCaloriesFromInputs([budgetNumberInput]);

          if (isError) {
            return;
          };

          const consumedCalories = breakfastCalories + lunchCalories + dinnerCalories + snacksCalories;
          const remainingCalories = budgetCalories - consumedCalories + exerciseCalories;

          const surplusOrDeficit = remainingCalories < 0 ? "Surplus" : "Deficit";
          output.innerHTML = `
          <span class="${surplusOrDeficit.toLowerCase()}">${Math.abs(remainingCalories)} Calorie ${surplusOrDeficit}</span>
          <hr>
          <p>${budgetCalories} Calories Budgeted</p>
          <p>${consumedCalories} Calories Consumed</p>
          <p>${exerciseCalories} Calories Burned</p>
          `;
          output.classList.remove('hide');
          };
  
### getCaloriesFromInputs()

This function helps retrieve the calorie values from input fields, cleaning the input to remove invalid characters and ensuring the values are numbers. If any invalid input is found, it alerts the user and stops further calculations.

Example:
Loops through each input field, removes invalid characters, and sums up valid calorie values.

    function getCaloriesFromInputs(list) {
        let calories = 0;
  
        for (const item of list) {
          const currVal = cleanInputString(item.value);
          const invalidInputMatch = isInvalidInput(currVal);
  
          if (invalidInputMatch) {
            alert(`Invalid Input: ${invalidInputMatch[0]}`);
            isError = true;
            return null;
          };
        }
        calories += Number(currVal);
        return calories;
        };

### clearForm()

This function clears all the user input fields and resets the output. It helps the user quickly start a new calculation with no leftover data from the previous session.

Example:
Clears input fields and hides the output section.

    function clearForm(){
      const inputContainer = document.querySelectorAll(".input-container");
      for(const container of inputContainers) {
        container.innerHTML = '';
      }
      budgetNumberInput.value = '';
      output.innerText = '';
      output.classList.add('hide');
    };

## Conclusion
The Calorie Counter project provides an intuitive and straightforward tool for users to manage their daily calorie intake and expenditure. With a simple yet effective interface, it allows individuals to track their meals, exercises, and overall progress towards their daily calorie goals.

By leveraging HTML, CSS, and JavaScript, this project demonstrates how to create an interactive and dynamic web application. The logic behind calculating calorie intake, tracking food and exercise entries, and displaying calorie surpluses or deficits is all handled seamlessly within the app.
