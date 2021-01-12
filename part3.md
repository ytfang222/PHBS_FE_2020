Final part is a notebook to reproduce partial results of the paper in the section 2 and section3. To sum up, the paper propose a new stochastic volatility model with Bayesian methods via Markov chan monte carlo(MCMC) methods.

We also give special credit to ChadFulton who provide the related dataset and code and insight us to do the simulation. This is a useful reference link for us to learn and coding.



//

For the empirical illustration in this notebook, we use a exchange rate dataset which contains USXUK, USXGER, USXJPN. We do log transformation and make one lag difference to see that transformed data in time series is stationary. The plot shows that it is smoothed.



//

Previously, Traditional stochastic volatility estimate method called Quasi-likelihood estimation is that the model is approximated by replacing log et^2 with a Gaussian random variable of the same mean and variance. However, the quasi-likelihood estimator under the assumption that loge_t^2 is normal has poor small sampler properties, even though the usual quasi-likelihood asymptotic theory is correct.

So the motivation of the paper as we previous mentioned is that to solve the problem of Quasi-likelihood estimation. To show the improvement of the MCMC model, we need to estimate both of them and then compare the parameters of them together. Here, we use the sv.QLSV provided by ChadFulton to fit the parameters of the Quasi-likelihood and estimate phi, sigma2_eta and mu. 

The result is shown as below. 



//

Later, we will show the MCMC methods can  improve the properties of stochastic  estimated parameter estimates. This notebook focuse on the mixture simulator in the paper sector 3.2.  not cover the integrating out the log-valtilities and reweighting model these two models.

The paper mentioned an alternative method for estimating this stochastic volatility model  which is called offset mixture of normals distribution to accurately approximate the exact likelihood. This approximation helps in the production of an efficient Monte Carlo procedure that to sample all the log-volatilities at once.

The iteration algorithms is already mentioned in our presention. The coding part is following the algorithm and implement it step by step. Firstly, we set the priors the sigma_yita^2, phi and mu. and then we do sampling from the conditional posteriors. The conditional posterior distribution is provided in the previous as our teammember mentioned. 

Secondly, we do the MCMC simulation. Below we perform 10,000 iterations to sample from the posterior. When presenting results,  we will drop the first 5,000 iterations as the burn-in period, and of the remaining 5,000 iterations we will save only every tenth iteration. Results will then be computed from the remaining 500 iterations.

ksc_params are we set for the simulation. We construct a class called TVLLDT, the funtion of this class is Time-varying local linear deterministic trend. 【Dont understant the class of TVLLDT...】

1. Convert to log squares, with offset
2. Initialize base model
3. Setup time-varying arrays for observation equation
4. Setup fixed components of state space matrices
5. update_mixing
6. update

Now, we start to do the simulation. 

1. we set the random seed first.

2. Setup the model and simulation smoother

3. Simulation parameters

4. Storage for traces

5. Initial values

6. Do iteration :

   ​	Update the parameters of the model

   ​	Simulation smoothing

   ​	Draw mixing indicators

   ​	Draw parameters

Finally, we show the results of the MCMC model comparing with the Quasi-likelihood Method Model.



//

We also want to know the properties of the sigma2. Since the parameter ση controls the variance of the underlying stochastic volatility process, underestimation will dampen changes in the volatility process over the sample. This is shown in the below graph. The figure plot the estimated stochastic volatility: posterior median and 15%/85% credible set. The organge line is the Quasi-likelihood estimation. The blue line is the posterior mean of the MCMC simulation, the shadow blue is the credible set of the posterior mean. The intuition of the plot is that the estimated  sigma^2 of QMLE will lower than the MCMC estimation.



//

As we want to know more about the estimated parameters. so we plot the distribution of parameters.  There are three hisgram of parameters of phi, sigma_yita, beta. For the result ,we can see that the sigma_yita of Quasi-likelihood estimation will lower than the MCMC mixture simulator. For phi and beta, the estimator of Quasi-likelihood estimation will higher than the MCMC simulator.

We want to use the result to interpre the formula which is provided in A canonical univariate stochastic volatility model. 

The parameters estimated by the two methods will sightly different from the log volatility in t+1 time.

ht+1 = u+phi(h_t - mu) + sigma_yita  yita_t