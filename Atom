namespace atom {
	real a0 = 5.1;
	int JC(int x)
	{
		int v = 1;
		for (int i = 1; i <= x; i++)
		{
			v *= i;
		}
		return v;
	}
	real laguerreL(int n, real x)
	{
		if (n <= 0)
			return 1;
		return (2 * n - 1 - x) * laguerreL(n - 1, x) - (n - 1) * (n - 1) * laguerreL(n - 2, x);
	}
	real laguerreL(int n, int m, real x)
	{
		if (n <= 0)
			return 1;

		return ((2 * n - 1 + m - x) * laguerreL(n - 1, m, x) - (n - 1 + m) * laguerreL(n - 2, m, x)) / n;
	}
	real PML(int m, int l, real x)
	{
		// PRINT("-----------------------");
		 //PRINT("m="<<m<<" l="<<l << " x="<<x)
		real A = pow(1 - x * x, m / 2);
		// PRINT("A=" << A);
		real sum = 0;
		for (int k = 0; k <= int((l - m) / 2); k++)
		{
			real jk = JC(k);
			real jk2 = JC(l - k);
			real jk3 = JC(l - 2 * k - m);
			// PRINT("K=" << k << " jk=" << jk << " jk2=" << jk2 << " jk3=" << jk3)

			real B = pow(2, l) * jk * jk2 * jk3;

			real E = pow(x, l - 2 * k - m);
			// PRINT("B=" << B << " E=" << E);
			sum += (pow(-1, k) * JC(2 * l - 2 * k) / B) * E;
		}
		return A * sum;
	}

	void atom()
	{
		{
			int n = 1 + rand() % 9;
			int l = rndi(0, n - 1);
			int m = rndi(-l, l);
			int ms = rand() % 2 == 0 ? -1 : 1;
      
			PRINT("n=" << n << " l=" << l);
			int cor = RNDCOR;
			for (int jj = 0; jj < 500; jj++)
			{
				real r = blend(0.1,500, jj / 500.0f);
				real R = 1;
				{
					real A = sqrt(pow(2 / (n * a0), 3) * ((real)JC(n - l - 1) / (2.0 * n * JC(n + l))));
					real B = pow(2 * r / (n * a0), l);
					real C = laguerreL(n - l - 1, 2 * l + 1, 2 * r / (n * a0));
					real E = pow(2.718281828459, -(r / (n * a0)));
					R = A * B * C * E;
					PRINT("r=" << r << " R=" << R << " C=" << C << " A=" << A << " B=" << B << " E=" << E);
				}
				real Y0 = (1 / sqrt(2 * PI)) * sqrt(((2 * l + 1) / 2.0) * ((real)JC(1 - m) / JC(1 + m)));
				PRINT("Y0=" << Y0);
				for (int ii = 0; ii <= 1000; ii++)
				{
					real ang = blend(0, 2 * PI, ii / 1000.0f);

					real pml = PML(m, l, cos(ang));
					real Yml = Y0* pml;
					//PRINT("Yml=" << Yml << " pml=" << pml);
					cor = blendcor(1, 0xFFffFFFF, fabs(Yml * R) * 10000.0f);

					vec2 p(r * cos(ang), r * sin(ang));
					pixel(p * 0.0005 + vec2(0.5, 0.5), cor);
					//PRINT("fabs(pr * R)=" << fabs(pr));
				}
			}
		}
	}
}
