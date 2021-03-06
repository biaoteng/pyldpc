.. note::
    :class: sphx-glr-download-link-note

    Click :ref:`here <sphx_glr_download_auto_examples_plot_coding_decoding_simulation.py>` to download the full example code
.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_examples_plot_coding_decoding_simulation.py:


================================================
Coding - Decoding simulation of a random message
================================================

This example shows a simulation of the transmission of a binary message
through a gaussian white noise channel with an LDPC coding and decoding system.


.. code-block:: default



    import numpy as np
    from pyldpc import make_ldpc, decode, get_message, encode
    from matplotlib import pyplot as plt

    n = 30
    d_v = 2
    d_c = 3
    seed = np.random.RandomState(42)






First we create an LDPC code i.e a pair of decoding and coding matrices
H and G. H is a regular parity-check matrix with d_v ones per row
and d_c ones per column


.. code-block:: default


    H, G = make_ldpc(n, d_v, d_c, seed=seed, systematic=True, sparse=True)

    n, k = G.shape
    print("Number of coded bits:", k)





.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

    Number of coded bits: 11



Now we simulate transmission for different levels of noise and
compute the percentage of errors using the bit-error-rate score


.. code-block:: default



    errors = []
    snrs = np.linspace(-2, 10, 20)
    v = np.arange(k) % 2  # fixed k bits message
    n_trials = 50  # number of transmissions with different noise
    for snr in snrs:
        error = 0.
        for ii in range(n_trials):
            y = encode(G, v, snr, seed=seed)
            d = decode(H, y, snr)
            x = get_message(G, d)
            error += abs(v - x).sum() / k
        errors.append(error / n_trials)

    plt.figure()
    plt.plot(snrs, errors, color="indianred")
    plt.ylabel("Bit error rate")
    plt.xlabel("SNR")
    plt.show()



.. image:: /auto_examples/images/sphx_glr_plot_coding_decoding_simulation_001.png
    :class: sphx-glr-single-img


.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

    /Users/hichamjanati/Documents/github/pyldpc/pyldpc/decoder.py:54: UserWarning: Decoding stopped before convergence. You may want
                           to increase maxiter
      to increase maxiter""")
    /Users/hichamjanati/Documents/github/pyldpc/examples/plot_coding_decoding_simulation.py:51: UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.
      plt.show()




.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 0 minutes  24.036 seconds)


.. _sphx_glr_download_auto_examples_plot_coding_decoding_simulation.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download

     :download:`Download Python source code: plot_coding_decoding_simulation.py <plot_coding_decoding_simulation.py>`



  .. container:: sphx-glr-download

     :download:`Download Jupyter notebook: plot_coding_decoding_simulation.ipynb <plot_coding_decoding_simulation.ipynb>`


.. only:: html

 .. rst-class:: sphx-glr-signature

    `Gallery generated by Sphinx-Gallery <https://sphinx-gallery.github.io>`_
