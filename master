import streamlit as st

# Define the function for calculating rent vs buy decision
def calculate_rent_vs_buy(
    home_price, deposit, loan_term, mortgage_rate, property_tax_rate,
    insurance_rate, maintenance_rate, home_appreciation_rate, rent, rent_increase_rate,
    investment_return_rate, years_to_compare):

    loan_amount = home_price - deposit
    monthly_interest_rate = mortgage_rate / 12
    total_payments = loan_term * 12
    monthly_mortgage_payment = loan_amount * (monthly_interest_rate * (1 + monthly_interest_rate) ** total_payments) / ((1 + monthly_interest_rate) ** total_payments - 1)

    annual_maintenance = home_price * maintenance_rate
    annual_property_tax = home_price * property_tax_rate
    annual_insurance = home_price * insurance_rate
    total_annual_cost_owning = annual_maintenance + annual_property_tax + annual_insurance + monthly_mortgage_payment * 12

    # Calculate the savings from renting if renting is more expensive
    savings_investment = deposit  # Initial investment of the deposit
    current_rent = rent
    for year in range(1, years_to_compare + 1):
        annual_rent = current_rent * 12
        if annual_rent > total_annual_cost_owning:
            # Only invest the difference if renting is more expensive
            savings_investment += (annual_rent - total_annual_cost_owning) * (1 + investment_return_rate) ** (years_to_compare - year + 1)
        current_rent *= (1 + rent_increase_rate)

    home_value = home_price * (1 + home_appreciation_rate) ** years_to_compare
    net_cost_buying = total_annual_cost_owning * years_to_compare - (home_value - loan_amount)
    total_cost_renting = annual_rent * years_to_compare

    if net_cost_buying < total_cost_renting - savings_investment:
        decision = "Buying is financially better."
    else:
        decision = "Renting is financially better."

    return decision, net_cost_buying, total_cost_renting - savings_investment, monthly_mortgage_payment

# Main block to create the Streamlit app
def main():
    st.title("Rent vs Buy Decision Calculator")

    st.sidebar.header("Input Parameters")

    home_price = st.sidebar.number_input("Home Price", value=300000)
    deposit = st.sidebar.number_input("Deposit", value=60000)
    loan_term = st.sidebar.number_input("Loan Term (years)", value=30)
    mortgage_rate = st.sidebar.number_input("Mortgage Rate (annual %)", value=3.5) / 100
    property_tax_rate = st.sidebar.number_input("Property Tax Rate (annual %)", value=1.2) / 100
    insurance_rate = st.sidebar.number_input("Home Insurance Rate (annual %)", value=0.5) / 100
    maintenance_rate = st.sidebar.number_input("Maintenance Rate (annual %)", value=1) / 100
    home_appreciation_rate = st.sidebar.number_input("Home Appreciation Rate (annual %)", value=2) / 100
    rent = st.sidebar.number_input("Current Monthly Rent", value=1500)
    rent_increase_rate = st.sidebar.number_input("Rent Increase Rate (annual %)", value=2) / 100
    investment_return_rate = st.sidebar.number_input("Investment Return Rate (annual %)", value=5) / 100
    years_to_compare = st.sidebar.number_input("Years to Compare", value=30)

    if st.sidebar.button("Calculate"):
        decision, net_cost_buying, net_cost_renting, monthly_mortgage_payment = calculate_rent_vs_buy(
            home_price, deposit, loan_term, mortgage_rate, property_tax_rate,
            insurance_rate, maintenance_rate, home_appreciation_rate, rent, rent_increase_rate,
            investment_return_rate, years_to_compare
        )

        st.subheader("Results")
        st.write(f"Decision: {decision}")
        st.write(f"Net Cost of Buying: ${net_cost_buying:,.2f}")
        st.write(f"Net Cost of Renting: ${net_cost_renting:,.2f}")
        st.write(f"Monthly Mortgage Payment: ${monthly_mortgage_payment:,.2f}")

if __name__ == "__main__":
    main()
