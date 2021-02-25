~~~~python
def quantize(x, L=127):
    old_name_suffix=x.name.split(':')[-1]
    new_name_suffix=re.sub(r'\d+','',old_name_suffix)
    new_name=':'.join(x.name.split(':')[:-1])+':'+new_name_suffix
    x.rename(new_name)
    
    name = x.name
    s_value = ParamProvider(name + ':s_value', initializer=[-5])
    s = SetGrad(s_value, None)
    s.rename(name + ':s_param')
    t = Pow(2, s)
    t.rename(name + ':scale')
    x = SetGrad(x, None)
    
    x_scaled = x / t
    x_scaled.rename(name + ':scaled')
    x_clipped = clip(x_scaled, -L-1, L)
    x_clipped.rename(name + ':clipped')
    x_rounded = Round(x_clipped).astype('int8')
    x_rounded.rename(name + ':rounded')
    x_flq = x_rounded * t
    x_flq.rename(name + ':flq')

    grad_x_flq = GradWrt(x_flq)
    mask_clip = (x_scaled < -1.5 - L) + (x_scaled > L + 0.5)
    mask_quant = Abs(mask_clip - 1)
    grad_quant = grad_x_flq * mask_quant * (x_rounded - x_scaled)
    grad_clip = grad_x_flq * mask_clip * x_rounded
    grad_s = grad_quant.sum() + grad_clip.sum()
    grad_s = grad_s * t * 0.7
    s.set_grad_var(grad_s)
    grad_x = grad_x_flq * mask_quant
    x.set_grad_var(grad_x)

    x = x_flq
    return x

~~~~



