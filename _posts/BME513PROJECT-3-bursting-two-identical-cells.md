```julia
using PyPlot
include("constants.jl")
include("tau_function.jl")
include("functions.jl")
```


```julia

```


```julia
# parameters
dt = .1;
beta =[0,0];

# initial values of v, m, n, s
v = [];
push!(v,-60)
m = [];
push!(m,tau_inf_m(v[end]))
n = [];
push!(n,tau_inf_n(v[end]))
s = [];
push!(s,tau_inf_s(v[end]))

# initail values
m_inf = tau_inf_m(v[end]);
n_inf = tau_inf_n(v[end]);
s_inf = tau_inf_s(v[end]);
```


```julia
# initialize the neurons

num_neurons = 2;
neurons = Array{neuron,1}(undef,num_neurons)
conn = [[2],[1]];

for i = 1:num_neurons
    neurons[i] = neuron(v,n,s,m_inf,n_inf,s_inf,conn[i],beta[i]);
end
```


```julia
beta = 0;
```


```julia
# iteration for simulation

for i = 1:1000000
    
    for num = 1:num_neurons
        

        # tau for each variable
        neurons[num].m_inf = tau_inf_m(neurons[num].voltage[end]);
        neurons[num].n_inf = tau_inf_n(neurons[num].voltage[end]);
        neurons[num].s_inf = tau_inf_s(neurons[num].voltage[end]);
    
        #m_inf = tau_inf_m(v[end]);
        #n_inf = tau_inf_n(v[end]);
        #s_inf = tau_inf_s(v[end]);
    
        # net currents
        
        ICa = ICa_current(neurons[num].voltage[end],neurons[num].m_inf);
        IK = IK_current(neurons[num].voltage[end],neurons[num].n_open[end]);
        IS = Is_current(neurons[num].voltage[end],neurons[num].s_open[end]);
        IG = 0;
        

        
        for co = 1:1
            diff = neurons[num].voltage[end] - neurons[neurons[num].connection[]].voltage[end];
            IG += gC*diff;
        end
        # net currents
        #ICa = ICa_current(v[end],m_inf);
        #IK = IK_current(v[end],n[end]);
        #IS = Is_current(v[end],s[end]);
        
        
        net = -ICa - IK - IS - IG;
    
        # update voltage
        new_v = neurons[num].voltage[end] + dt*net/tau;
        push!(neurons[num].voltage,new_v)
    
        # update n
        g = lamda*(neurons[num].n_inf - neurons[num].n_open[end]); 
        new_n = neurons[num].n_open[end] + dt*g/tau;
        push!(neurons[num].n_open,new_n)
    
        # update s
        h = neurons[num].s_inf - neurons[num].s_open[end];
        new_s = neurons[num].s_open[end] + dt*(h +beta)/tau_s;
        push!(neurons[num].s_open,new_s)
    end
end
```


```julia

```


```julia
plot(neurons[1].voltage)
plot(neurons[2].voltage)
xlim(200000,450000)

```


    
![png](output_7_0.png)
    





    (200000, 450000)




```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```


```julia

```
