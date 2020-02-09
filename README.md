# HP-Filter
Hodrick-Prescott filter with the option to use either the standard two-sided 
    or one-sided implementation. The two-sided implementation leads to equivalent
    results as when using the statsmodel.tsa hpfilter function
    
    Parameters
    ----------
    X : array-like
        The time series to filter (1-d), need to add multivariate functionality.
        
    side : int
           The implementation requested. The function will default to the standard
           two-sided implementation.
           
    smooth : float 
            The Hodrick-Prescott smoothing parameter. A value of 1600 is
            suggested for quarterly data. Ravn and Uhlig suggest using a value
            of 6.25 (1600/4**4) for annual data and 129600 (1600*3**4) for monthly
            data. The function will default to using the quarterly parameter (1600).
    freq : str
           Optional parameter to specify the frequency of the data. Will override
           the smoothing parameter and implement using the suggested value from
           Ravn and Uhlig. Accepts annual (a), quarterly (q), or monthly (m)
           frequencies.
    Returns
    -------
    
    cycle : ndarray
            The estimated cycle in the data given side implementation and the 
            smoothing parameter.
            
    trend : ndarray
            The estimated trend in the data given side implementation and the 
            smoothing parameter.
    
    References
    ----------
    Hodrick, R.J, and E. C. Prescott. 1980. "Postwar U.S. Business Cycles: An
        Empirical Investigation." `Carnegie Mellon University discussion
        paper no. 451`.
        
    Meyer-Gohde, A. 2010. "Matlab code for one-sided HP-filters."
        `Quantitative Macroeconomics & Real Business Cycles, QM&RBC Codes 181`.
    
    Ravn, M.O and H. Uhlig. 2002. "Notes On Adjusted the Hodrick-Prescott
        Filter for the Frequency of Observations." `The Review of Economics and
        Statistics`, 84(2), 371-80.
    
    Examples
    --------
    from statsmodels.api import datasets, tsa
    import pandas as pd
    dta = datasets.macrodata.load_pandas().data
    index = pd.DatetimeIndex(start='1959Q1', end='2009Q4', freq='Q')
    dta.set_index(index, inplace=True)
    
    #Run original tsa.filters two-sided hp filter
    cycle_tsa, trend_ts = tsa.filters.hpfilter(dta.realgdp, 1600)
    #Run two-sided implementation
    cycle2, trend2 = hprescott(dta.realgdp, 2, 1600)
    #Run one-sided implementation
    cycle1, trend1 = hprescott(dta.realgdp, 1, 1600)
