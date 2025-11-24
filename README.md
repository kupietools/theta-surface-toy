
# Theta & Delta vs DTE – Strike from |Delta| at Reference DTE (BSM)

This project is an interactive visualization of Black–Scholes-Merton option Theta and Delta as a function of days to expiration (DTE), for a strike defined by a chosen absolute Delta (|Delta|) on position opening date.

In plain language: you pick an underlying price, a target Delta and a reference DTE (for example, “40Δ at 30 DTE”) representing the DTE of the option when you open the position (buy or sell the option). The tool solves for the strike that gives you that Delta at that DTE, then shows you how Theta or Delta evolve as time passes and DTE shrinks to zero.

The app is useful for:

- Understanding term‑structure behavior of Theta and Delta.
- Seeing how volatility, interest rates, and dividends impact time decay.
- Exploring call vs. put symmetry in Black–Scholes.
- Getting intuition for how a specific “Delta slice” behaves as an option ages.

<img width="1280" height="818" alt="Screen Shot 2025-11-23 at 8 40 50 PM" src="https://github.com/user-attachments/assets/5fa87b4a-7d7d-4b30-a4e9-749646c2a84d" />


---

## Live Demo

If you’re viewing this from the original repository, you can open the app directly by loading: `thetasurface.html` in any modern browser.

---

## Conceptual Overview

The visualization is built around the Black–Scholes–Merton (BSM) model.

### 1. Defining the Strike from |Delta| at a Reference DTE

You choose:

- Underlying price S. You can define the min and max of the range slider. 
- Target absolute Delta |Delta| at a reference maturity  - the delta the option position is opened at
- Reference DTE (days) - the Days Till Expiration the option position is opened at.

The app then solves for the strike K for both a call and put, as is needed by the the BSM formula, such that the option has the chosen |Delta| at the chosen reference DTE. The calculated strikes are displayed.

This captures the idea of a Delta slice such as “40Δ at 30DTE” and then tracks that strike over time.

---

### 2. Market Inputs & Chart Horizon

You can configure market parameters needed by BSM and the maximum time horizon:

- Volatility sigma (“Vol σ”) 
- Risk‑free interest rate r (“Rate r”) 
- Dividend yield q (“Div q”) 
- Max DTE (chart)  
  Controls the left edge of the chart (largest DTE).  
  The X‑axis represents the option’s own days to expiration (DTE). The X‑axis runs from Max DTE → 0, i.e. from “farther from expiration” to “expiration”.

---

### 3. Theta & Delta Graph Options

At the top right of the chart you can switch between:

- Theta per day vs Days‑to‑Expiration
- Delta vs Days‑to‑Expiration

---

### 4. "Probe" DTE & Theta Statistics

There is a movable "probe" line within the chart:

- Probe DTE (days)  
  Must be between 0 and Max DTE.

At the current probe DTE, the tool displays:

- Call theta per day @ probe DTE
- Put theta per day @ probe DTE
- Call theta per day @ ATM (K = S)
- Put theta per day @ ATM (K = S)

All values are per 1 unit of underlying.  
For short positions, you simply flip the sign.

This makes it straightforward to compare:

- Theta for your fixed‑strike Delta slice vs.
- Theta for an at‑the‑money (ATM) option at the same DTE.

---

## How to Use

1. Open `thetasurface.html` in a modern browser (Chrome, Firefox, Edge, Safari).
2. In the “Underlying & |Delta| Slice” panel:
   - Choose an “Underlying S” (and/or bounds).
   - Select a “|Delta| at reference DTE” (for example 0.40).
   - Set “Reference DTE (days)” (for example 30).
   - Note the computed “K from |Δ| @ reference DTE (call/put)”.
3. In the “Market Inputs & Max DTE” panel:
   - Set Vol σ, Rate r, Div q.
   - Set Max DTE for the chart horizon.
4. At the top right of the chart:
   - Choose Theta or Delta.
   - Choose Call or Put.
5. Use the “Probe DTE” slider to inspect:
   - Theta and Delta values at specific DTE.
   - Comparison vs ATM at that same DTE.

---

## Typical Use Cases

- Options education – teaching how Theta accelerates as expiration approaches, and how Delta changes for a fixed strike.
- Strategy design – understanding behavior of “40Δ calls at 30 days” or similar commonly referenced slices.
- Risk visualization – intuition for how time decay differs between calls/puts, different vols, and different yield/interest scenarios.

---

## Files

`thetasurface.html` is a self‑contained HTML file with the entire interactive visualization (including any embedded JavaScript and CSS).

There are no build steps required; the page is designed to be opened directly in a browser.

---

## Running Locally

1. Clone or download the repository.
2. Open the file `thetasurface.html` in your browser.

---

## Mathematical Background (Sketch)

The tool uses the standard Black–Scholes–Merton formulas for European-style options.

Let:

- S = underlying price  
- K = strike price  
- T = time to maturity (in years)  
- r = risk‑free interest rate  
- q = continuous dividend yield  
- sigma = volatility (standard deviation of log returns)  
- N(x) = cumulative distribution function (CDF) of the standard normal

Then:

- Call Delta is calculated as: exp(−q * T) * N(d1)

- Put Delta is calculated as: −exp(−q * T) * N(−d1)

where:

- d1 = [ ln(S / K) + (r − q + 0.5 * sigma^2) * T ] / (sigma * sqrt(T))  
- d2 = d1 − sigma * sqrt(T)

Theta (per year) is computed from the usual Black–Scholes closed‑form expressions for call and put prices and then converted to “per day” by appropriate scaling (for example, dividing by 365).

To define the strike for a chosen absolute Delta |Delta| at the reference DTE T_ref, the app numerically inverts the Delta formula to solve for K such that:

- For a call slice:  
  |Delta_call(S, K, T_ref)| = chosen |Delta|

- For a put slice:  
  |Delta_put(S, K, T_ref)| = chosen |Delta|

That strike K is then treated as fixed while T varies along the X‑axis as DTE decreases from Max DTE down to 0.

---

## Contributing

If you want to:

- Extend the visualization,
- Add alternative models (for example local volatility or stochastic volatility),
- Improve the UI/UX or documentation,

you’re welcome to fork the repository and submit pull requests.

---

## License

Included in this repo as LICENSE.

---

## Acknowledgments

This visualization is based on the Black–Scholes–Merton framework and is inspired by practical option‑trading conventions such as quoting positions by Delta at a reference DTE (for example “40Δ at 30DTE”) and then tracking their evolution over time. 

This is based on a code framework generated by prompting an LLM. 
