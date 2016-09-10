=======================================================
[PyDLM]
=======================================================

Welcome to [pydlm], a flexible, user-friendly and rich functionality time series modeling library for python. This package implementes the Bayesian dynamic linear model for time series data. All modeling functionality is integrated in the `dlm`
```
  >>> #import dlm and its modeling components
  >>> from pydlm import dlm, trend, seasonality, dynamic, autoReg
  >>>
  >>> #randomly generate data
  >>> data = [0] * 100 + [3] * 100
  >>>
  >>> #construct the base
  >>> myDLM = dlm(data)  
  >>>
  >>> #adding model components
  >>> myDLM = myDLM + trend(2, name = 'lineTrend') #add a second-order trend (linear trending)
  >>> myDLM = myDLM + seasonality(7, name = 'day7') #add a 7 day seasonality
  >>> myDLM = myDLM + autoReg(3, name = 'ar3') #add a 3 step auto regression
  >>>
  >>> #show the added components
  >>> myDLM.ls()
  >>>
  >>> #delete unwanted component
  >>> myDLM.delete('ar3')
  >>> myDLM.ls()
  >>>
  >>> #fit forward filter
  >>> myDLM.fitForwardFilter()
  >>>
  >>> #fit backward smoother
  >>> myDLM.fitBackwardSmoother()
  >>>
  >>> #plot the results
  >>> myDLM.plot()
  >>>
  >>> #plot only the filtered results
  >>> myDLM.turnOff('smoothed plot')
  >>> myDLM.plot()
  >>>
  >>> #plot in one figure
  >>> myDLM.turnOff('multiple plots')
  >>> myDLM.plot()
```
It supports missing observations
```
  >>> data = [1, 0, 0, 1, 0, 0, None, 0, 1, None, None, 0, 0]
  >>> myDLM = dlm(data) + trend(2)
  >>> myDLM.fit() #fit() will fit both forward filter and backward smoother
```
It also includes the discounting factor, which can be used to control how rapid the model should adapt to the new data
```
  >>> data = [0] * 100 + [3] * 100
  >>> myDLM = dlm(data) + trend(2, discount = 1.0)
  >>> myDLM.fit()
  >>> myDLM.plot()
  >>>
  >>> myDLM.delete('trend')
  >>> myDLM = myDLM + trend(2, discount = 0.9)
  >>> myDLM.fit()
  >>> myDLM.plot()
  >>>
  >>> # get the filtered and smoothed results
  >>> filteredObs = myDLM.getFilteredObs()
  >>> smoothedObs = myDLM.getSmoothedObs()
```
For online updates
```
  >>> myDLM = dlm([]) + trend(2) + seasonality(7)
  >>> for t in range(0, len(data)):
  ...     myDLM.append([data[t]])
  ...     myDLM.fitForwardFilter()
  >>> filteredObs = myDLM.getFilteredObs()
```  
------------
Installation
------------

For now you can get the latest and greatest from [github]
(https://github.com/wwrechard/PyDLM)

      $ git clone git@github.com:wwrechard/PyDLM.git PyDLM
      $ cd PyDLM
      $ sudo python setup.py install

In the future (after adding the multivariate case), the package will
be up on `PyPI`. 

`pydlm` depends on the following modules,

* `numpy`     (for core functionality)
* `Sphinx`    (for generating documentation)
* `unittest`  (for testing)
