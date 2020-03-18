常见如下两种格式是环境贴图
# 1.cubeMap由六张贴图组成的环境贴图

![image](https://learnopengl-cn.github.io/img/04/06/cubemaps_skybox.png)
# 2.hdr环境贴图
![image](https://learnopengl-cn.github.io/img/07/03/01/ibl_hdr_radiance.png)

```


void convert_cube_uv_to_xyz(int index, float u, float v, float *x, float *y, float *z)
{
	// convert range 0 to 1 to -1 to 1
	float uc = 2.0f * u - 1.0f;
	float vc = 2.0f * v - 1.0f;
	switch (index)
	{
	case 0: *x = 1.0f; *y = vc; *z = -uc; break;	// POSITIVE X  right
	case 1: *x = -1.0f; *y = vc; *z = uc; break;	// NEGATIVE X  left
	case 2: *x = uc; *y = 1.0f; *z = -vc; break;	// POSITIVE Y  top 
	case 3: *x = uc; *y = -1.0f; *z = vc; break;	// NEGATIVE Y  bottom 
	case 4: *x = uc; *y = vc; *z = 1.0f; break;	// POSITIVE Z    front
	case 5: *x = -uc; *y = vc; *z = -1.0f; break;	// NEGATIVE Z back
	}
}
```
```
void convert_xyz_to_cube_uv(float x, float y, float z, int *index, float *u, float *v)
{
	float absX = fabs(x);
	float absY = fabs(y);
	float absZ = fabs(z);

	int isXPositive = x > 0 ? 1 : 0;
	int isYPositive = y > 0 ? 1 : 0;
	int isZPositive = z > 0 ? 1 : 0;

	float maxAxis, uc, vc;

	// POSITIVE X
	if (isXPositive && absX >= absY && absX >= absZ) {
		// u (0 to 1) goes from +z to -z
		// v (0 to 1) goes from -y to +y
		maxAxis = absX;
		uc = -z;
		vc = y;
		*index = 0;
	}
	// NEGATIVE X
	if (!isXPositive && absX >= absY && absX >= absZ) {
		// u (0 to 1) goes from -z to +z
		// v (0 to 1) goes from -y to +y
		maxAxis = absX;
		uc = z;
		vc = y;
		*index = 1;
	}
	// POSITIVE Y
	if (isYPositive && absY >= absX && absY >= absZ) {
		// u (0 to 1) goes from -x to +x
		// v (0 to 1) goes from +z to -z
		maxAxis = absY;
		uc = x;
		vc = -z;
		*index = 2;
	}
	// NEGATIVE Y
	if (!isYPositive && absY >= absX && absY >= absZ) {
		// u (0 to 1) goes from -x to +x
		// v (0 to 1) goes from -z to +z
		maxAxis = absY;
		uc = x;
		vc = z;
		*index = 3;
	}
	// POSITIVE Z
	if (isZPositive && absZ >= absX && absZ >= absY) {
		// u (0 to 1) goes from -x to +x
		// v (0 to 1) goes from -y to +y
		maxAxis = absZ;
		uc = x;
		vc = y;
		*index = 4;
	}
	// NEGATIVE Z
	if (!isZPositive && absZ >= absX && absZ >= absY) {
		// u (0 to 1) goes from +x to -x
		// v (0 to 1) goes from -y to +y
		maxAxis = absZ;
		uc = -x;
		vc = y;
		*index = 5;
	}

	// Convert range from -1 to 1 to 0 to 1
	*u = 0.5f * (uc / maxAxis + 1.0f);
	*v = 0.5f * (vc / maxAxis + 1.0f);
}
```