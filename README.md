难免不向银行借钱，随手写了一个计算器。
通过比较发现，两种贷款方式前五年是差不多的，第六年才开始有比较大差距

```
public class AAAA {

	public static void main(String[] args) {

		doit(101, 4.9 / 100 / 12, 30 * 12);
	}

	// 总额 a 万, 月利率 p(日利率*30 或者 年利率/12), 总月数 n,前x年的利息差， 付款总额差
	private static void doit(double a, double p, int n) {
		a *= 10000;

		double sben = 0, sli = 0;
		double sben2 = 0, sli2 = 0;

		System.out.println("(等额本息/等额本金)");
		for (int i = 0; i < n; i++) {
			if (i % 12 == 0)
				System.out.println("第" + (i / 12 + 1) + "年");
			int j = i + 1;// 第j月

			// ap(1+p)^(i-1)/((1+p)^12n – 1)
			double ben = a * p * Math.pow(1 + p, j - 1) / (Math.pow(1 + p, n) - 1);// 本金
			sben += ben;

			// ap((1+p)^12n-(1+p)^(i-1))/((1+p)^12n– 1)
			double li = a * p * (Math.pow(1 + p, n) - Math.pow(1 + p, j - 1)) / (Math.pow(1 + p, n) - 1); // 利息
			sli += li;

			// 等额本息
			// ########################################################################################################
			// 等额本金

			// a/(12n)
			double ben2 = a / n;// 本金
			sben2 += ben2;

			// ap(1-(i-1)/(12n))
			double li2 = a * p * (1 - ((double) j - 1) / n); // 利息
			sli2 += li2;

			System.out.println(String.format("%2d:	(%.2f / %.2f)        (%.2f / %.2f)		(%.2f / %.2f) ",
					j % 12 == 0 ? 12 : j % 12, ben, ben2, li, li2, ben + li, ben2 + li2));

			if (j % 12 == 0)
				System.out.println(String.format("===>>> 等客本息 - 等额本息: %.2f万 , %.2f万 , %.2f万", (sben - sben2) / 10000,
						(sli - sli2) / 10000, ((sben + sli) - (sben2 + sli2)) / 10000));
		}

		System.out.println(String.format("%2s:	(%.2f 万/ %.2f万)    (%.2f万 / %.2f万)    (%.2f万 / %.2f万) ", "总的",
				sben / 10000, sben2 / 10000, sli / 10000, sli2 / 10000, (sben + sli) / 10000, (sben2 + sli2) / 10000));
	}

}

```
