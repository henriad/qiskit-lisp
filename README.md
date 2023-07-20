# qiskit-lisp
;;Load the CFFI library
(require : cffi)

;; Define a foreign library for Qiskit
(cffi:defcffi-module qiskit
		     ((:python "from qiskit impor QuantumCircuit, execute, Aer\n"
			       ""from qiskit.visualization import plot_bloch_multivector, plot_histogram\n")))

(defparameter *python-script* "
from qiskit import QuantumCircuit, transpile, assemble, Aer

def run_quantum_circuit():
    qc = QuantumCircuit(2, 2)
    qc.h(0)
    qc.cx(0, 1)
    qc.measure([0, 1], [0, 1])

    backend = Aer.get_backend('qasm_simulator')
    t_qc = transpile(qc, backend)
    qobj = assemble(t_qc)
    result = backend.run(qobj).result()

    counts = result.get_counts(qc)
    print(counts)
")

(defun run-qiskit-example ()
  (python:with-python (*standard-output* :stream)
    (python:py-formatter "exec" *python-script*)))

(run-qiskit-example)


