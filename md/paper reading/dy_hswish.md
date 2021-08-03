~~~~python
def DY_hswish(inp, delta_ab, prefix='DY_hswish'):
  # inp: (N, C, H, W)
  # delta_ab: (N,2*K)
  ab = np.zeros((1, K, 2))
  ab[0,0,0] = 1.0
  ab[0,2,1] = 6.0
  ab = ConstProvider(ab, name='ab') # (1, K, 2)
  with GroupNode(prefix).context_reg():
    inp_p3 = inp + 3
    delta_ab = delta_ab.reshape(-1, K, 2)
    alpha_beta = ab + delta_ab # (N, K, 2)
    alpha_beta.rename(alpha_beta.name + ':alpha_beta')
    alpha = alpha_beta[:, :, 0]
    beta = alpha_beta[:, :, 1]

    out0 = alpha[:, 0].reshape(-1,1,1,1)*inp_p3 + beta[:, 0].reshape(-1,1,1,1)
    out1 = alpha[:, 1].reshape(-1,1,1,1)*inp_p3 + beta[:, 1].reshape(-1,1,1,1)
    out2 = alpha[:, 2].reshape(-1,1,1,1)*inp_p3 + beta[:, 2].reshape(-1,1,1,1)
    out = Min(Max(out0, out1), out2)
    return inp / 6 * out
~~~~

