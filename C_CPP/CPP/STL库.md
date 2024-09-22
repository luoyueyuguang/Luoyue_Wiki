## std::experimental::simd
``` cpp
template < class T, class Abi >
class simd
SIMD类型的统一抽象。T即元素类型，Abi即底层表示。类内重载了各种运算符。

template < class T, class Abi >
class simd_mask
掩码类型，用于条件运算，布尔运算符已重载

using float_v = native_simd<float>;

float_v foo(float_v a, float_v b)
{
	return a + b;
}
```
>`simd_abi :: scalar`:simd<T, scalar> 等同于 T。
>`simd_abi :: native <T>`:最高效的类型
>`simd_abi :: fixed_size <N>`:指定长度的类型， 不保证翻译单元间兼容。
>`simd_abi :: compatible <T>`: 保证翻译单元间ABI兼容。
>`simd_abi :: deduce <T, N>`:根据长度推导出合适的类型， 目前实现是优先选择 scalar 和 native, 长度不匹配的话 fallback 到 fixef_size。

在 -march = skylake-avx512 下：
>`simd_abi :: native <T>`:AVX512
>`simd_abi :: fixed_size <N>`:fixed_size<16>但多了一些冗余指令。
>`simd_abi :: compatible <T>`: SSE
>`simd_abi :: deduce <T, N>`:`decuce_t<T, 16> == native<T>, 
>`deduce_t<T, 32> == fixed_size<32>

>`element_aligned`:按aligonof(T)对齐，T = typeof(T)。
>`vector_aligned`:memory_alignment_v<T, U>类型对齐， 目前实现是bit_ceil(sizeof(U) * N), T = SIMD类型， U = buffer元素类型， N = simd元素个数。
>`overaligned_tag<N>`:按字节对齐

``` cpp
template < class M, class V >
class const_where_expression;

template < class M, class V >
class where_expression

//examples
where(mask, x) = y;
mask为true的位置, x对应的值会被y对应的值覆盖
```

>`[static_]simd_cast/ to_{fixed_size, compatible, native} / split / contact`:类型转换
>`min / max / minmax / clamp/ <cmath>`:作用域每个元素
>`reduce / h{min, max} / {fall, any, none, some}_of / popcount / find_{first, last}_set_`:
>折叠成单个元素。