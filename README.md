
#include <stdio.h>

#define MODES 4
#define MONTH_DAYS 30

// Transportation modes
const char *modeNames[MODES] = {
    "Motorcycle", "Car", "Bus", "Train"
};

// Emission factors
const double transportFactors[MODES] = {
    0.103, 0.171, 0.089, 0.041
};

const double ELECTRICITY_FACTOR = 0.70;
const double LPG_FACTOR = 3.00;
const double WASTE_FACTOR = 0.50;

// Function to read non-negative input
double getInput(char message[]) {
    double value;
    int ch;

    while (1) {
        printf("%s", message);

        if (scanf("%lf", &value) == 1 && value >= 0) {
            while ((ch = getchar()) != '\n' && ch != EOF);
            return value;
        }

        printf("Invalid input! Enter a valid non-negative number.\n");

        while ((ch = getchar()) != '\n' && ch != EOF);
    }
}

// Calculate transportation emissions
double transportEmission(double distance, double factor) {
    return distance * factor;
}

// Calculate electricity emissions
double electricityEmission(double units) {
    return units * ELECTRICITY_FACTOR;
}

// Calculate cooking emissions
double cookingEmission(double lpg) {
    return lpg * LPG_FACTOR;
}

// Calculate waste emissions
double wasteEmission(double waste) {
    return waste * WASTE_FACTOR;
}

// Calculate total emissions
double totalEmission(double t, double e, double c, double w) {
    return t + e + c + w;
}

// Calculate yearly emissions
double yearlyEmission(double monthly) {
    return monthly * 12;
}

// Display footprint category
void classifyFootprint(double total) {

    printf("\n----------------------------------------\n");
    printf("        CARBON FOOTPRINT STATUS\n");
    printf("----------------------------------------\n");

    if (total < 100) {
        printf("Category : LOW\n");
        printf("Message  : Excellent! Keep it up.\n");
    }
    else if (total < 300) {
        printf("Category : MODERATE\n");
        printf("Message  : Try to reduce your emissions.\n");
    }
    else {
        printf("Category : HIGH\n");
        printf("Message  : Adopt more eco-friendly habits.\n");
    }
}

// Display percentage contribution
void displayPercentage(double total, double t,
                       double e, double c, double w) {

    printf("\n----------------------------------------\n");
    printf("       EMISSION CONTRIBUTION (%%)\n");
    printf("----------------------------------------\n");

    if (total == 0) {
        printf("No emissions recorded.\n");
        return;
    }

    printf("Transportation : %.2f%%\n", (t / total) * 100);
    printf("Electricity    : %.2f%%\n", (e / total) * 100);
    printf("Cooking Fuel   : %.2f%%\n", (c / total) * 100);
    printf("Waste          : %.2f%%\n", (w / total) * 100);
}

// Personalized suggestions
void suggestReduction(double t, double e,
                      double c, double w) {

    printf("\n----------------------------------------\n");
    printf("        ECO-FRIENDLY SUGGESTIONS\n");
    printf("----------------------------------------\n");

    if (t >= e && t >= c && t >= w)
        printf("* Use public transport, carpool or cycle.\n");

    if (e >= t && e >= c && e >= w)
        printf("* Switch off unused appliances and use LED lights.\n");

    if (c >= t && c >= e && c >= w)
        printf("* Use cooking fuel efficiently.\n");

    if (w >= t && w >= e && w >= c)
        printf("* Recycle, reuse and compost waste.\n");

    printf("* Avoid single-use plastic products.\n");
    printf("* Follow sustainable daily habits.\n");
}

// Compare transportation modes
void compareTravel(double distance, int currentMode) {

    int i;
    double current, alternative;

    printf("\n========================================\n");
    printf("       TRANSPORT COMPARISON\n");
    printf("========================================\n");

    current = transportEmission(
        distance, transportFactors[currentMode]
    );

    printf("Current Mode : %s\n", modeNames[currentMode]);
    printf("Current CO2  : %.2f kg\n", current);

    for (i = 0; i < MODES; i++) {

        if (i != currentMode) {

            alternative = transportEmission(
                distance, transportFactors[i]
            );

            printf("\nAlternative : %s\n", modeNames[i]);
            printf("CO2 Emission: %.2f kg\n", alternative);

            if (alternative < current) {
                printf("CO2 Saved   : %.2f kg\n",
                       current - alternative);
            }
            else {
                printf("No emission savings.\n");
            }
        }
    }
}

// NEW FEATURE: Carbon reduction calculator
void reductionCalculator(double t, double e,
                         double c, double w) {

    int choice;
    double percentage, saving, newTotal;
    double total = totalEmission(t, e, c, w);
    double source;

    printf("\n========================================\n");
    printf("       CARBON REDUCTION CALCULATOR\n");
    printf("========================================\n");

    printf("Choose the category to reduce:\n");
    printf("1. Transportation\n");
    printf("2. Electricity\n");
    printf("3. Cooking Fuel\n");
    printf("4. Waste\n");

    do {
        printf("\nEnter your choice (1-4): ");

        if (scanf("%d", &choice) != 1) {
            int ch;
            while ((ch = getchar()) != '\n' && ch != EOF);
            choice = 0;
        }

    } while (choice < 1 || choice > 4);

    switch (choice) {
        case 1:
            source = t;
            break;

        case 2:
            source = e;
            break;

        case 3:
            source = c;
            break;

        case 4:
            source = w;
            break;
    }

    percentage = getInput(
        "Enter reduction percentage (0-100): "
    );

    if (percentage > 100) {
        printf("Maximum reduction is 100%%. Setting it to 100%%.\n");
        percentage = 100;
    }

    saving = source * percentage / 100;
    newTotal = total - saving;

    printf("\n----------------------------------------\n");
    printf("          REDUCTION RESULTS\n");
    printf("----------------------------------------\n");

    printf("Current Monthly Emission : %.2f kg CO2\n", total);
    printf("Potential CO2 Savings    : %.2f kg CO2\n", saving);
    printf("New Monthly Emission     : %.2f kg CO2\n", newTotal);
    printf("New Yearly Estimate      : %.2f kg CO2\n",
           yearlyEmission(newTotal));

    printf("----------------------------------------\n");
    printf("Great! Every small change makes a difference.\n");
}

// Main results dashboard
void displayDashboard(double t, double e,
                      double c, double w) {

    double total = totalEmission(t, e, c, w);

    printf("\n\n");
    printf("========================================\n");
    printf("          ECOTRACK DASHBOARD\n");
    printf("========================================\n");

    printf("\n         MONTHLY EMISSION REPORT\n");
    printf("----------------------------------------\n");

    printf("%-20s : %10.2f kg CO2\n", "Transportation", t);
    printf("%-20s : %10.2f kg CO2\n", "Electricity", e);
    printf("%-20s : %10.2f kg CO2\n", "Cooking Fuel", c);
    printf("%-20s : %10.2f kg CO2\n", "Waste", w);

    printf("----------------------------------------\n");
    printf("%-20s : %10.2f kg CO2\n", "TOTAL MONTHLY", total);
    printf("%-20s : %10.2f kg CO2\n",
           "DAILY AVERAGE", total / MONTH_DAYS);
    printf("%-20s : %10.2f kg CO2\n",
           "YEARLY ESTIMATE", yearlyEmission(total));

    printf("========================================\n");

    classifyFootprint(total);

    displayPercentage(total, t, e, c, w);

    suggestReduction(t, e, c, w);

    printf("\nThank you for choosing a greener future!\n");
}

// Main function
int main() {

    int mode, choice, i;

    double distance, travelDays;
    double electricityUnits, lpg, waste;

    double transport, electricity;
    double cooking, wasteValue;

    printf("\n");
    printf("========================================\n");
    printf("    ECOTRACK - PERSONAL CARBON CALCULATOR\n");
    printf("========================================\n");

    printf("\nCalculate your monthly carbon footprint!\n");

    // Transportation details
    printf("\n------------ TRANSPORTATION ------------\n");

    for (i = 0; i < MODES; i++) {
        printf("%d. %s\n", i + 1, modeNames[i]);
    }

    do {
        printf("Select travel mode (1-4): ");

        if (scanf("%d", &mode) != 1) {
            int ch;
            while ((ch = getchar()) != '\n' && ch != EOF);
            mode = 0;
        }

    } while (mode < 1 || mode > 4);

    mode--;

    distance = getInput(
        "Daily travel distance (km): "
    );

    travelDays = getInput(
        "Travel days per month (0-30): "
    );

    if (travelDays > 30) {
        printf("Maximum travel days set to 30.\n");
        travelDays = 30;
    }

    transport = transportEmission(
        distance * travelDays,
        transportFactors[mode]
    );

    // Electricity details
    printf("\n-------------- ELECTRICITY -------------\n");

    electricityUnits = getInput(
        "Monthly electricity consumption (kWh): "
    );

    electricity = electricityEmission(electricityUnits);

    // Cooking fuel details
    printf("\n-------------- COOKING FUEL ------------\n");

    lpg = getInput(
        "Monthly LPG consumption (kg): "
    );

    cooking = cookingEmission(lpg);

    // Waste details
    printf("\n---------------- WASTE -----------------\n");

    waste = getInput(
        "Monthly waste generated (kg): "
    );

    wasteValue = wasteEmission(waste);

    // Display dashboard
    displayDashboard(
        transport, electricity, cooking, wasteValue
    );

    // Additional features menu
    do {

        printf("\n========================================\n");
        printf("             ECOTRACK MENU\n");
        printf("========================================\n");

        printf("1. Compare transportation modes\n");
        printf("2. Calculate potential carbon reduction\n");
        printf("3. Exit\n");

        printf("----------------------------------------\n");

        printf("Enter your choice: ");

        if (scanf("%d", &choice) != 1) {
            int ch;
            while ((ch = getchar()) != '\n' && ch != EOF);
            choice = 0;
        }

        switch (choice) {

            case 1:
                compareTravel(distance * travelDays, mode);
                break;

            case 2:
                reductionCalculator(
                    transport, electricity,
                    cooking, wasteValue
                );
                break;

            case 3:
                printf("\nThank you for using ECOTRACK!\n");
                printf("GO GREEN, SAVE THE PLANET!\n");
                break;

            default:
                printf("\nInvalid choice! Please select 1-3.\n");
        }

    } while (choice != 3);

    printf("\n========================================\n");

    return 0;
}
