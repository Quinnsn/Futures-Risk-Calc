# Futures-Risk-Calc
import streamlit as st
import math

st.set_page_config(page_title="Futures Risk Calculator", page_icon="📉", layout="centered")

st.title("📉 Futures Risk Calculator")
st.markdown("Figure out how many contracts to trade based on your stop size and risk %.")

# === Inputs ===
account_balance = st.number_input("Account Balance ($)", value=1000)
risk_percent = st.slider("Risk % per Trade", min_value=1, max_value=100, value=25)
stop_size_points = st.number_input("Stop Size (in Points)", value=25.0)
contract_type = st.selectbox("Contract", ["MNQ (Micro Nasdaq)", "MES (Micro S&P)", "NQ (E-mini Nasdaq)", "ES (E-mini S&P)"])

# === Tick Values ===
contract_info = {
    "MNQ (Micro Nasdaq)": {"tick_value_per_point": 2.0, "ticks_per_point": 4},
    "MES (Micro S&P)": {"tick_value_per_point": 5.0, "ticks_per_point": 4},
    "NQ (E-mini Nasdaq)": {"tick_value_per_point": 20.0, "ticks_per_point": 4},
    "ES (E-mini S&P)": {"tick_value_per_point": 50.0, "ticks_per_point": 4}
}

tick_value_per_point = contract_info[contract_type]["tick_value_per_point"]
ticks_per_point = contract_info[contract_type]["ticks_per_point"]

# === Calculations ===
total_risk = account_balance * (risk_percent / 100)
stop_ticks = stop_size_points * ticks_per_point
tick_value_per_tick = tick_value_per_point / ticks_per_point
risk_per_contract = stop_ticks * tick_value_per_tick
contracts = math.floor(total_risk / risk_per_contract)

# === Output ===
st.markdown("---")
st.subheader("🧾 Trade Sizing Result")

st.write(f"**Stop Size:** {stop_size_points} points = {int(stop_ticks)} ticks")
st.write(f"**Tick Value:** ${tick_value_per_tick:.2f} per tick")
st.write(f"**Risk per Contract:** ${risk_per_contract:.2f}")
st.write(f"**Contracts to Trade:** `{contracts}`")

st.markdown("---")
st.caption("Built by you + ChatGPT ⚡")
