import pandas as pd


def calculate_crs(row):
    """
    Career Resilience Score (CRS)
    CRS = (Income Stability + Barrier + Judgment) - Automation Risk
    """
    return (
        row["income_stability"]
        + row["barrier_to_entry"]
        + row["judgment_requirement"]
        - row["automation_risk"]
    )


def categorize_risk(score):
    """
    Categorize career based on CRS score
    """
    if score >= 20:
        return "Highly Resilient"
    elif score >= 15:
        return "Resilient"
    elif score >= 10:
        return "Moderate"
    else:
        return "High Risk"


def main():
    print("\n=== AI Career Survival Guide (2026) ===\n")

    # Load dataset
    df = pd.read_csv("careers.csv")

    # Calculate CRS
    df["CRS"] = df.apply(calculate_crs, axis=1)

    # Categorize careers
    df["Category"] = df["CRS"].apply(categorize_risk)

    # Sort by CRS
    df_sorted = df.sort_values(by="CRS", ascending=False)

    # Display Top 10
    print("Top 10 Most Resilient Careers:\n")
    print(df_sorted[["career", "CRS", "Category"]].head(10).to_string(index=False))

    # Display Bottom 5
    print("\nBottom 5 High-Risk Careers:\n")
    print(df_sorted[["career", "CRS", "Category"]].tail(5).to_string(index=False))

    # Save results
    df_sorted.to_csv("career_rankings_output.csv", index=False)

    print("\nFull ranking saved as 'career_rankings_output.csv'\n")


if __name__ == "__main__":
    main()
