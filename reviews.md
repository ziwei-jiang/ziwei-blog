
# Review for Calibrated Propensities for Causal Effect Estimation 

# Review
## Summary
The paper presents an intuitive and straightforward technique for propensity score model calibration. The authors effectively motivate the need for calibration in Inverse Propensity Treatment Weighting (IPTW) and provide theoretical analysis of the calibrated propensity score model. An calibration algorithm is proposed and demonstrated with IPTW and AIPW models. The experiments shows the calibrated model improves the estimate from IPTW and AIPW and outperform many SOTA method. 

## Pros:
- The paper thoroughly motivates the problem and provides solid theoretical analysis of the calibrated propensity score model.
- The conducted experiments are well-designed, effectively showcasing the advantages of the calibrated model. The authors also demonstrate that their calibrated IPTW estimator matches or outperforms other state-of-the-art methods for GWAS analysis.

## Cons:


## Minor details:
- l.146l Should "expressive neural Q" be "expressive neural network Q"?
- l.128r What does "R\cir H" refer to in the context of the model H?
- The proposed calibration method seems to achieve better performance with the IPWT model compared to the AIPW model in general. Is there any intuition provided for this observation?
- Theorem 3.5 presents a theoretical guarantee for the calibrated propensity score model under the separability condition. While the paper provides a nice intuition, it is not entirely clear how achievable this condition is. It would be beneficial to discuss this further and potentially provide experimental evidence.


Overall, this paper is well-written and presents a motivated problem with solid theoretical analysis. The experiments are well-conducted and effectively showcase the proposed method.

# Review for Results on Counterfactual Invariance

# Review 

## Summary 
The paper discusses the relationships between different definitions of counterfactual invariance and their connections to causal graphs and conditional independence. The author demonstrates that when building counterfactually invariant predictors from observational data, it is challenging to outperform the existing method implied by graph structures.




## Pros:
- The paper provides rigorous theoretical analysis, establishing connections between various definitions of counterfactual invariance and conditional independence.
- The paper's structure is well-constructed, with a logical flow of ideas.
- The analysis leads to a nice conclusion regarding the construction of counterfactually invariant predictors using observational data.
- Several examples are provided, helping readers understand the intuition of the theorems.

## Minor details:
- l.70r Is the definition presented in Kusner et al. 2017 equivalent to Definition 2.2 in the paper?

Overall, I believe this paper offers valuable insights into different definitions of counterfactual invariance and their relationship to conditional independence. This knowledge is useful for constructing counterfactually invariant predictors from observational data.


# Review for Delphic Offline Reinforcement Learning under Nonidentifiable Hidden Confounding



## Summary
This paper introduces a method for estimating three different types of uncertainties in reinforcement learning. The authors address the issue of structural bias caused by hidden confounding variables by defining delphic uncertainty and proposing a novel method to measure it in practice. The proposed algorithm is demonstrated with extensive experiments using simulated and real-world data.

## Pros:
- The paper addresses a novel and practical problem in reinforcement learning.
- The introduction of delphic uncertainty establishes a useful connection between reinforcement learning and causal inference.
- The proposed method for handling confounding bias is innovative and does not require parametric assumptions for the functions, making it applicable to various scenarios.
- The paper presents extensive experiments that demonstrate the effectiveness of the proposed method in improving offline reinforcement learning when hidden confounders are present.


I think this paper provides a valuable problem and draw connection between reinforcement learning and causal inference. The algorithm presented in the paper is novel and construction is rigorous. 