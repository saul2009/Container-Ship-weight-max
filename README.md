
Container ship weight-maximization

# Pseudocode for greedy_max_weight

// Compute the optimal set of cargo items with a greedy algorithm.
// Specifically, among the cargo items that fit within a total_volume space,
// choose the goods whose weight-per-volume is greatest.
// Repeat until no more cargo items can be chosen, either because we've run out of cargo items,
// or run out of space.

std::unique_ptr<CargoVector> greedy_max_weight
(
	const CargoVector& goods,
	double total_volume
)
{

	auto current = goods;
	std::unique_ptr<CargoVector> result(new CargoVector);
	double result_volume = 0.0;

	while (!current.empty())

	{
		double max = current[0]->weight() / current[0]->volume();
		size_t max_index = 0;

		for (size_t i = 0; i < current.size(); i++)
		{
			if (max < (current[i]->weight() / current[i]->volume()))
			{
				<find max value within that subset>
			}
		}

		double v = current[max_index]->volume(); // a's volume

		auto a = current[max_index];			 // a

		if ((result_volume + v) <= total_volume)
		{
			<push value into vector and add to total volume(v)>
		}

		<Make list shorter by Erasing from current> Erase a from current
	}
	return result
}

#Analysis of greedy_max_weight

S.C = 2 + ( (while loop) (for loop* block ) (if condition + max(if,else))
S.C = 2 <while loop makes list shorter through each loop therefore log n>
S.C = 2 + log n * (for loop(if statement) + if statement)
S.C = 2 + log n * n((3 + 3 ) + 2 + (2 + 2))
S.C = 2 + log n * n(6)+2 + 4
S.C = 2 +log n * 6n + 6
S.C = log n * 6n + 8
Time complexity => O(n log n)



# Pseudocode for exhaustive_max_weight

// Compute the optimal set of cargo items with a exhaustive search algorithm.
// Specifically, among all subsets of cargo items, return the subset whose volume
// in cubic meters fits within the total_volume budget and whose total weight is greatest.
// To avoid overflow, the size of the cargo items vector must be less than 64.

std::unique_ptr<CargoVector> exhaustive_max_weight
(
	const CargoVector& goods,
	double total_volume
)
{
const double x = goods.size();
assert(x < 64);
int64_t VW_size = pow(2, x);
double  weighttotalcc, volumetotalcc, volumetotalopt, weighttotalopt;
std::unique_ptr<CargoVector> opt(new CargoVector);
for (int64_t bits = 0; bits < VW_size; bits++)
{
	std::unique_ptr<CargoVector> cc(new CargoVector);

	for (int i = 0; i < x; i++)
	{
		if ((bits >> i) & 1)
		{
			cc-> push_back (std::shared_ptr<CargoItem>(new CargoItem(
				goods.at(i)->description(),
				goods.at(i)->volume(),
				goods.at(i)->weight())
			)
		);
		}
	}

	sum_cargo_vector(*cc, volumetotalcc, weighttotalcc) ;


	if (volumetotalcc <= total_volume)
	{
		sum_cargo_vector(*opt,  volumetotalopt, weighttotalopt);
		if (opt ->empty() || weighttotalopt  < weighttotalcc)
		{
			opt = filter_cargo_vector(*cc, 1, 4000, cc->size());
		}
	}

}

return opt;

}


#Analysis of exhaustive_max_weight

S.C = (for loop *(for loop * block) + 2 + n + if(condition) + n+ 2 + if(condition) + n + 6)
S.C = (since the set is a subset of itself, the two for loops become 2^n)
S.C = (2^n) + 2 + n + 1 + 2 + n + 2 + n + 6)
S.C = (2^n * 3n + 13)
Therefore, time complexity => O(2^n * n)

S.C = 2^n*n should be expected result
