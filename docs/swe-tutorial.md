# Best Practices for using Sensor Web Technology

This tutorial has been developed within the
[ConnectinGEO](http://connectingeo.net/) project in order
to lower the hurdle to create interoperable solutions using Sensor Web
technology. It gives an overview on the relevant standards and the related
technology. In order to give the users a jump start, it uses exemplary
scenarios to illustrate the different approaches.


## Acknowledgement

<details>
  <summary>ConnectinGEO is funded by the Horizon 2020 Framework Program</summary>
  <p><br/>

![ec logo](images/ec.png "EC Logo")
<div>
ConnectinGEO is funded by the Horizon 2020 Framework Program for Research
and Innovation (SC5-18a-2014-H2020) of the European Union under grant
agreement number 641538.
</div>
  </p>
</details>

## Copyright Notice

<details>
  <summary>Copyright © 2016, ConnectinGEO Consortium</summary>
  <div><br/>

The ConnectinGEO Consortium grants third parties the right to use and
distribute all or parts of this document, provided that the ConnectinGEO
project and the document are properly referenced.

THIS DOCUMENT IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
DOCUMENT, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.</div>
</details>

## Exemplary Use Case Scenario

In times of climate change and global warming, the exploitation of renewable
resources is of high relevance. In particular, solar energy may be collected
and transformed into electricity with the help of solar plants. In this context,
the Surface Solar Irradiance (SSI) is an indicator for the potential of solar
energy. Via associated SSI measurements, suitable locations for installation of
new as well as monitoring of existing solar plants becomes possible. In
addition, when collecting SSI measurements of the same location over a period of
time, the energy output from solar irradiation can be forecast with respect to
storage and planning (Menard n.y). For this reason, SSI data can measured in
several ways, e.g. in-situ pyranometric sensors, satellite image processing or
numerical weather models. Each of these options may be defined as a sensor,
which takes certain inputs and produces SSI data as output. However, each sensor
uses a different access interface as well as input- and output definitions. As a
result, researchers and consumers of the SSI data have to deal with
heterogeneous sensor interfaces, making it hard to combine measurements of
different sources.

To overcome this heterogeneity, standardized formats and services have to be
employed for interoperable exchange of SSI data. According to Menard et al.
(2015), the Sensor Web Enablement (SWE) Framework offers this very
functionality, open standards for sensor data encoding and exchange as well as
several Web services to homogeneously access sensor data from heterogeneous
sensors. Within this Best Practice Guide, the central components of the SWE
framework are introduced to provide basic knowledge on how to use them.
