# Retirement Certificate Support

Carbonmark connects with a variety of carbon registries and works to harmonize data to provide our buyers with a seamless experience while browsing the market.&#x20;

Importantly, each registry platform runs on its own technology stack and the industry has not yet moved to consolidate the underlying infrastructure in a way that unlocks true interoperability. As such, the feature set that we can provide varies depending on where a credit was issued.

We have provided an overview of key functionality per registry below. This information will be updated as developments continue both at Carbonmark and with our registry partners.&#x20;

| Registry                            | Carbonmark Support                                    | Retirement Fractionalization Enabled | Retirement Certificate Mirroring |
| ----------------------------------- | ----------------------------------------------------- | ------------------------------------ | -------------------------------- |
| \*Verra                             | Yes, legacy transferred credits only as of 10/14/2026 | Yes - 0.001 tonnes                   | No                               |
| Gold Standard                       | No                                                    | n/a                                  | n/a                              |
| International Carbon Registry (ICR) | Yes                                                   | Yes - 0.001 tonnes                   | Yes                              |
| EcoRegistry                         | Yes                                                   | No - whole tonnes only               | Yes                              |
| Puro.Earth                          | Yes                                                   | No - whole tonnes only               | Yes                              |
| Regen Registry                      | Yes                                                   | Yes - 0.001 tonnes                   | \*\*Batched                      |
| T-VER                               | Yes                                                   | Yes - 0.001 tonnes                   | \*\*Batched                      |
| Universal Carbon Registry (UCR)     | Yes                                                   | Yes - 0.001 tonnes                   | \*\*Batched                      |
| OneShot.Earth                       | In Development                                        | No - whole tonnes only               | Yes                              |
| Social Carbon                       | In Development                                        | No - whole tonnes only               | Yes                              |

**\*Legacy VCS credits on Carbonmark**

Credits from Verra's VCS carbon certification standard were transferred to our platform using a one-way process to avoid double counting as Verra's registry infrastructure did not support transfers of credits outside of its system.

The one-way process utilized was akin to the process undertaken when converting CDM certified credits to VCS or Gold Standard, whereby credits were cancelled in the host registry and then re-issued on a separate system.

In our case, legacy VCS credits currently listed on Carbonmark were cancelled in Verra's registry and re-issued on our underlying distributed ledger system by Toucan Protocol. Therefore, these credits cannot be sent back to a Verra registry account.

Importantly, the environmental benefit of these credits was not claimed during the original transfer process, and we provide traceability tooling to link all VCS credits on our platform to the original transfer event on Verra's registry.

**\*\*Batched retirement certificate mirroring**&#x20;

Certain credit types on our platform can be fractionally retired on Carbonmark to enable their utility for high-volume, low-tonnage retirements through our API (e.g. for small-scale e-commerce or supply-chain retirements). These individual retirements are then aggregated into whole tonne units for retirement in the underlying registry. Evidence for these batched retirements is hosted on the project pages (in the case of TVER) or is made available via our credit provenance tooling on the retirement certificate pages.&#x20;
