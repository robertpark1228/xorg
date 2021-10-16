# SigprofilerExtractor 사용법





from SigProfilerExtractor import sigpro as sig

```
from SigProfilerExtractor import sigpro as sig
sig.sigProfilerExtractor("vcf","sig_result_p33_CDDO5nm","/disk1/oneomics_data/KNIH_DATA/omics_analysis_p/wgrs/sig_p33",reference_genome="GRCh38", opportunity_genome = "GRCh38", context_type = "default", exome = False, minimum_signatures=1, maximum_signatures=10, nmf_replicates=100, resample = True, batch_size=1, cpu=-1, gpu=False, nmf_init="random", precision= "single", matrix_normalization= "gmm", seeds= "random", min_nmf_iterations= 10000, max_nmf_iterations=1000000, nmf_test_conv= 10000, nmf_tolerance= 1e-15,nnls_add_penalty=0.05, nnls_remove_penalty=0.01, initial_remove_penalty=0.05, de_novo_fit_penalty=0.02,get_all_signature_matrices= False)
```

## ​

​

