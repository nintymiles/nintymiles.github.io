如何在UE custom node中使用function

```
struct MyFunc{
    float plot(float2 st, float pct){
      return smoothstep( pct-0.02, pct, st.y) -
        function smoothstep( pct, pct+0.02, st.y);
    }
    
};

float2 st = position/resolution;

// Smooth interpolation between 0.1 and 0.9
float y = smoothstep(0.1,0.9,st.x);

float3 color = float3(y,y,y);

MyFunc mf;
float pct = mf.plot(st,y);
color = (1.0-pct)*color+pct*float3(0.0,1.0,0.0);

return float4(color,1.0);


float2 st = position/resolution;

// Smooth interpolation between 0.1 and 0.9
float y = smoothstep(0.1,0.9,st.x);                       
float3 color = float3(y,y,y);

//float pct = plot(st,y);                                                      
float pct = smoothstep( y-0.02, y, st.y) - smoothstep( y, y+0.02, st.y);
color = (1.0-pct)*color+pct*float3(0.0,1.0,0.0);

return float4(color,1.0); }                                   
float plot(float2 st, float pct){
  return  smoothstep( pct-0.02, pct, st.y) -
          smoothstep( pct, pct+0.02, st.y);  
          
          
```





```
struct MyFunc{
    float plot(float2 st, float pct){
      return smoothstep( pct-0.02, pct, st.y) -
         smoothstep( pct, pct+0.02, st.y);
    }
    
    //  Function from Iñigo Quiles
    //  www.iquilezles.org/www/articles/functions/functions.htm
    float cubicPulse( float c, float w, float x ){
        x = abs(x - c);
        if( x>w ) return 0.0;
        x /= w;
        return 1.0 - x*x*(3.0-2.0*x);
    }
};

float2 st = position/resolution;
MyFunc mf;
float y = mf.cubicPulse(0.5,0.2,st.x);

float3 color = float3(y,y,y);
float pct = mf.plot(st,y);
color = (1.0-pct)*color+pct*float3(0.0,1.0,0.0);

return float4(color,1.0);
```

