 // specify those channels we're going to use to communicate with streamtools
13	13	 type FFT struct {
14	14	 	blocks.Block
15		-	queryrule  chan chan interface{}
16		-	querystate chan chan interface{}
17		-	queryfft   chan chan interface{}
18		-	inrule     chan interface{}
19		-	inpoll     chan interface{}
20		-	in         chan interface{}
21		-	out        chan interface{}
22		-	quit       chan interface{}
15	+	queryrule chan chan interface{}
16	+	queryfft  chan chan interface{}
17	+	inrule    chan interface{}
18	+	inpoll    chan interface{}
19	+	in        chan interface{}
20	+	out       chan interface{}
21	+	quit      chan interface{}
23	22	 }
24	23	 
25		-type tsDataPoint struct {
26		-	Timestamp float64
27		-	Value     float64
28		-}
29		-
30		-type tsData struct {
31		-	Values []tsDataPoint
32		-}
33		-
34		-func fft(data tsData) {
24	+func buildFFT(data tsData) [][]float64 {
35	25	 	x := make([]float64, len(data.Values))
36	26	 	for i, d := range data.Values {
37	27	 		x[i] = d.Value
...	...	@@ -147,12 +137,12 @@ func (b *FFT) Run() {
147	137	 			}
148	138	 		case respChan := <-b.queryfft:
149	139	 			out := map[string]interface{}{
150		-				"fft": fft(data),
140	+				"fft": buildFFT(data),
151	141	 			}
152	142	 			respChan <- out
153	143	 		case <-b.inpoll:
154	144	 			out := map[string]interface{}{
155		-				"fft": fft(data),
145	+				"fft": buildFFT(data),
156	146	 			}
157	147	 			b.out <- out
