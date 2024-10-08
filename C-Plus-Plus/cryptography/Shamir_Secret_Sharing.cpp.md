```cpp
/* Shamir's Secret Sharing algorithm:
 * Generates a specified number of shares
 * where putting together a fixed number of
 * a subset of those shares reveals an encoded
 * secret value.
 */

#include<iostream>
#include<vector>
#include<map>
#include<random>
#include<type_traits>
using namespace std;

// Chosen RNG engine: mersenne twister
mt19937 rng;
// Global seed for chosen RNG engine
mt19937::result_type seed_val;

/* Initialise chosen RNG with seed value that is
 * entropy taken from system's entropy source
 * (generally /dev/urandom on Linux).
 */
void rng_init() {
    random_device rand_dev;
    seed_val = rand_dev();
    rng.seed(seed_val);
}

/* Populate a vector with specified number
 * of random natural numbers generated by chosen RNG.
 */
vector<unsigned> gen_rand_uints(unsigned num) {
    rng_init();

    // Use random values from a uniform distribution
    uniform_int_distribution<unsigned> uint_dist;
    vector<unsigned> rand_ints;
    for (unsigned i = 0; i < num; i++) {
        rand_ints.push_back(uint_dist(rng));
    }

    return rand_ints;
}

/* Generate a map from share number to share value for
 * Shamir's Secret Sharing algorithm, using the specified
 * secret natural number that is to be protected.
 *
 * The shares are a set of points which map from values of
 * x to values of f(x), where f(x) is a polynomial of
 * degree (min_shares - 1), and f(0) = secret.
 */
map<unsigned, unsigned> sss_gen_shares(unsigned min_shares, unsigned max_shares, unsigned secret) {
    vector<unsigned> poly_coeffs = gen_rand_uints(min_shares - 1);
    map<unsigned, unsigned> shares;
    for (unsigned x = 1; x <= max_shares; x++) {
        unsigned y = secret; // f(0)
        for (unsigned i = 1; i <= poly_coeffs.size(); i++) {
            unsigned term = poly_coeffs[i] * unsigned(pow(x, i));
            y += term;
        }
        shares[x] = y;
    }

    return shares;
}

/* Recover the secret encoded by Shamir's Secret Sharing
 * algorithm given a map of share number to share value.
 * The secret is given by f(0), which can be calculated
 * using any (min_shares - 1) points out of the total
 * (max_shares) points available.
 *
 * Taking min_shares = k, max_shares = n,
 * f(0) is calculated from (x_1, y_1), (x_2, y_2), ..., (x_k, y_k)
 * by using Lagrange polynomials to find the constant term given by
 * sum{i=0 to k-1} (y_i * (product{j=0 to k-1, i != j} (x_j/(x_j - x_i))))
 */
unsigned sss_recover_secret(const map<unsigned, unsigned>& shares) {
    double sum_of_terms = 0.0;
    for (auto i : shares) {
        unsigned temp_numerator = 1;
        int temp_denominator = 1; // int, since negative values possible
        for (auto j : shares) {
            if (i != j) {
                unsigned x_i = i.first;
                unsigned x_j = j.first;
                temp_numerator *= x_j;
                temp_denominator *= x_j - x_i;
            }
        }
        unsigned y_i = i.second;
        double term = double(y_i * temp_numerator) / temp_denominator;
        sum_of_terms += term;
    }
    unsigned secret = round(sum_of_terms);

    return secret;
}

int main() {
    unsigned min_shares;

    /* Less than 3 shares is impractical for security purposes,
     * since the constant term of the polynomial can then
     * be easily calculated.
     */
    do {
        cout << "Enter minimum number of shares to unlock secret (at least three): ";
        cin >> min_shares;
    } while (min_shares < 3);

    unsigned max_shares;
    do {
        cout << "Enter total number of shares to generate (not less than the minimum): ";
        cin >> max_shares;
    } while (max_shares < min_shares);

    unsigned secret;
    /* Beyond 1E9, the recovered secret key comes out incorrect as
     * calculation of the polynomial points overflow silently
     */
    do {
        cout << "Enter secret number (a natural number): ";
        cin >> secret;
    } while(secret >= 1E9);

    auto shares = sss_gen_shares(min_shares, max_shares, secret);
    cout << endl;
    cout << "Secret_shares (share number, share value):" << endl;
    for (auto share : shares) {
        cout << "(" << share.first << ", " << share.second << ")" << endl;
    }

    cout << endl;
    cout << "Enter any " << min_shares << " shares:" << endl;
    map<unsigned, unsigned> input_shares;
    for (unsigned i = 1; i <= min_shares; i++) {
        cout << "For input number " << i << ":" << endl;

        unsigned temp_x;
        cout << "    Share number: ";
        cin >> temp_x;

        unsigned temp_y;
        cout << "    Share value: ";
        cin >> temp_y;

        input_shares[temp_x] = temp_y;
    }
    auto recovered_secret = sss_recover_secret(input_shares);
    cout << endl;
    cout << "Recovered secret: " << recovered_secret << endl;
}

/* Example session:
Enter minimum number of shares to unlock secret (at least three): 4
Enter total number of shares to generate (not less than the minimum): 7
Enter secret number (a natural number): 123456789

Secret_shares (share number, share value):
(1, 3351437161)
(2, 1499306213)
(3, 3156998537)
(4, 4029546837)
(5, 4116951113)
(6, 3419211365)
(7, 1936327593)

Enter any 4 shares:
For input number 1:
    Share number: 7
    Share value: 1936327593
For input number 2:
    Share number: 2
    Share value: 1499306213
For input number 3:
    Share number: 3
    Share value: 3156998537
For input number 4:
    Share number: 1
    Share value: 3351437161

Recovered secret: 123456789
 */

/* For total number of shares = n
 * and minimum number of shares = k,
 * generation of shares has
 * Time Complexity = O(n),
 * and recovery of secret value has
 * Time Complexity = O(k^2).
 *
 * Overall program has
 * Space Complexity = O(k).
 */

```