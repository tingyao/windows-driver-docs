---
title: Point, Line, and Triangle Filling Requirements
description: Point, Line, and Triangle Filling Requirements
ms.assetid: 1a0a8160-01e2-4fb7-b1a2-6b61f1021fb9
keywords: ["point fill rule WDK Direct3D", "line fill rule WDK Direct3D", "triangle fill rules WDK Direct3D", "filling lines WDK Direct3D", "filling points WDK Direct3D", "filling triangles WDK Direct3D"]
---

# Point, Line, and Triangle Filling Requirements


## <span id="ddk_point_line_and_triangle_filling_requirements_gg"></span><span id="DDK_POINT_LINE_AND_TRIANGLE_FILLING_REQUIREMENTS_GG"></span>


The requirements for filling points, lines, and triangles are as follows:

### <span id="points"></span><span id="POINTS"></span> Points

The point fill and rasterization rules determine how a point is rendered. These rules are identical to the triangle fill rules. All flags and capabilities that apply to triangles also apply to points, and vice-versa. The reference implementation expands a point into a rectangle and applies the triangle fill rules to the result.

Given a point with coordinates P₀(x,y), generate four new points P₁, P₂, P₃, and P₄ as follows:

```
P1(x,y) = (x âˆ’ 0.5, y âˆ’ 0.5)
P2(x,y) = (x âˆ’ 0.5, y + 0.5)
P3(x,y) = (x + 0.5, y + 0.5)
P4(x,y) = (x + 0.5, y âˆ’ 0.5)
```

The rectangle is then generated as two triangles, such as (P₁, P₂, P₃) and (P₁, P₃, P₄). You may also examine the reference rasterizer implementation for rendering points in the source files *setup.cpp* and *scancnv.cpp* of the DirectX Driver Development Kit (DDK).

### <span id="lines"></span><span id="LINES"></span>Lines

Line fill rules (that is, rules that determine how a line is rendered) follow the Grid Intersection Quantization (GIQ) diamond convention. For more information about the GIQ diamond convention, see [Cosmetic Lines](cosmetic-lines.md). An example of line-drawing code that follows these rules can be found in the DirectX DDK in the reference rasterizer source files *setup.cpp* and *scancnv.cpp*.

### <span id="triangles"></span><span id="TRIANGLES"></span>Triangles

The triangle fill rules determine how a triangle is rendered. These rules are identical to the point fill rules. An example of triangle-drawing code that follows the triangle fill rules can be found in the DirectX DDK in the reference rasterizer source files *setup.cpp* and *scancnv.cpp*.

Hardware should supply the culling caps and properly implement the three culling modes. The following code fragment determines whether to cull the current triangle:

```
if (CurrentCullMode != D3DCULL_NONE) {
    int ccw = (((v[0]->sx - v[2]->sx) *
                (v[1]->sy - v[2]->sy)) <
               ((v[1]->sx - v[2]->sx) *
                (v[0]->sy - v[2]->sy)));
if ((CurrentCullMode == D3DCULL_CW && (ccw == 0)) ||
        (CurrentCullMode == D3DCULL_CCW && (ccw != 0))) {
        // Current triangle is culled, move onto
        // next triangle.
    }
}
// Current triangle is not culled, render it
```

The preceding code sample tests to determine which way the triangle is facing. The triangle is defined 0,1,2 and tested for being counterclockwise in screen space. If it is not, and there is clockwise culling, then that triangle is not drawn because the vertices go in clockwise order.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Point,%20Line,%20and%20Triangle%20Filling%20Requirements%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



